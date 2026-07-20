Got it — one classic pipeline, split into 3 YAML pipelines. That changes the approach: it's not just syntax conversion, it's decomposition, so no tool does the whole job end to end — but there is one worth using for the raw conversion step.

Tool: yamlizr

It's a .NET global tool built specifically because Microsoft's own "Export to YAML" button only works on Classic Build pipelines, not Release. yamlizr converts Classic Designer Build/Release definitions (including any referenced Task Groups) en masse into YAML equivalents, which is exactly the gap you have.

dotnet tool install --global yamlizr
yamlizr --help

Run it against your release definition export and it'll give you raw YAML for every stage/task — correct task names, versions, and inputs pulled straight from the definition, not guessed by an LLM. That solves the "high risk of AI hallucinating task versions on a complex pipeline" problem you hit with Copilot.

What it won't do — this is still your job after:

Won't decide how to split into 3 pipelines — that's a structural decision, not a conversion
Won't convert approvals — those move to Environment checks in ADO, not YAML syntax at all
Won't restructure sequential classic stages into dependsOn: [] parallel blocks — it converts what's there, it doesn't redesign the dependency graph
Won't dedupe repeated deploy logic across your lower envs into a template

So the realistic workflow:

4. Sequence

Convert Sonar template first (smallest, no dependencies).
Convert PR pipeline, validate it runs and gates the branch correctly.
Convert main pipeline stage-by-stage — one env at a time — checking each stage's parallelism/conditions against the JSON inventory before moving to the next.
Move approvals to Environment checks (Pipelines > Environments > Checks) — this is the most commonly missed step, since classic pre-deployment approvals don't map into YAML syntax at all.
Run classic and YAML in parallel until every lower env has deployed clean via YAML at least once before retiring classic.

If you want, share the instructions.md content and the JSON export (redacted of secrets/connection strings) here and I'll draft the actual template files with you stage by stage.

Run yamlizr against the full classic pipeline export → get raw YAML per stage/task. Treat this as your parts bin, not a finished pipeline.
Manually triage the output into 3 buckets based on your instructions.md: build/test steps → PR pipeline, Sonar task block → its own pipeline (templatized), deploy stages → main pipeline.
In the main pipeline, replace repeated per-env deploy stages with one parametrized deploy-env.yml template, called once per environment.
Rebuild parallelism explicitly with dependsOn (empty array for stages that ran in parallel in classic, chained for sequential ones) — yamlizr's output will likely be linear since classic's parallelism was often deployment-group-based, not stage-based.
Move approvals to Environment checks manually — no tool does this for you.
Do this in Claude Code locally against the yamlizr output and your instructions.md — that gives you a model working against real structured YAML files instead of trying to reverse-engineer complexity from a UI description, which is likely why Copilot choked.

That combination — yamlizr for faithful task-level extraction, then AI-assisted restructuring against that ground truth rather than against the classic UI directly — has a materially better success rate than asking an AI to convert-and-restructure in one pass.
