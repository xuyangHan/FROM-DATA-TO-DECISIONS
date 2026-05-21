# Domain 5 - CI/CD (practice)

Use these to test whether you can choose the right Git workflow, bundle configuration pattern, deployment command, and production identity from short exam-style scenarios.

## Questions and sample answers

1. A developer should not commit directly to `main`. What workflow should they use in Databricks Git folders/Repos?

   **Sample answer:** Pull the latest `main`, create a feature branch, make changes, commit and push the branch, then open a pull request for review before merge.

2. The exam says "Databricks Repos," but the docs say "Git folders." Are these different for the purpose of this domain?

   **Sample answer:** Treat them as the same concept: a Databricks workspace folder connected to a remote Git repository.

3. A developer edits a notebook in a Databricks Git folder but only commits locally. Can CI/CD see the change?

   **Sample answer:** No. The developer must push the commit to the remote branch so the PR and CI/CD system can access it.

4. A production job should use reviewed code only. Should a user edit the production notebook directly in the workspace?

   **Sample answer:** No. They should make changes on a branch, use a PR, and deploy from the merged branch through CI/CD.

5. What is the role of a pull request in a Databricks CI/CD workflow?

   **Sample answer:** It provides code review and a merge gate before changes are promoted to deployment branches such as `main` or `release`.

6. A team wants most users to run production workflows but not edit production source files. What Git folder pattern fits?

   **Sample answer:** Use a restricted production Git folder outside personal user folders, mapped to the deployment branch, with edit access limited to admins or service principals.

7. A developer starts work from an old local branch and later gets conflicts. What Git step was likely missed?

   **Sample answer:** Pulling the latest changes from the remote default branch before starting or before merging.

8. A job points to a personal workspace notebook that is not in Git. What is the CI/CD problem?

   **Sample answer:** The notebook is not governed by source control and PR review, so it is hard to promote consistently.

9. `dev_main` and `prod_main` catalog names differ across environments. Which bundle feature should you use?

   **Sample answer:** Use a bundle variable such as `${var.catalog}` with target-specific values under `targets.dev.variables` and `targets.prod.variables`.

10. Same job definition is needed in dev, test, and prod. Should you create three unrelated jobs manually in the UI?

    **Sample answer:** No. Define one job resource in a Declarative Automation Bundle and deploy it to different targets.

11. What file must exist at the root of a bundle?

    **Sample answer:** `databricks.yml`.

12. What mapping can `databricks.yml` use to load resource YAML files such as `resources/jobs.yml`?

    **Sample answer:** The `include` mapping.

13. How do you reference a custom bundle variable named `catalog`?

    **Sample answer:** `${var.catalog}`.

14. How do you reference the selected bundle target in YAML?

    **Sample answer:** `${bundle.target}`.

15. What is the difference between bundle variables and job parameters?

    **Sample answer:** Bundle variables are deployment-time configuration, such as environment catalog names. Job parameters are run-time values, such as a processing date for a specific run.

16. A job needs today's `process_date` when it runs. Should that be a bundle variable or a job parameter?

    **Sample answer:** A job parameter, because it changes per run.

17. A bundle should deploy to different workspace URLs for dev and prod. Where should this be configured?

    **Sample answer:** In target-specific `workspace.host` settings under `targets.dev` and `targets.prod`.

18. Only prod should have an unpaused schedule. What bundle feature fits?

    **Sample answer:** Use a target-specific resource override for the prod target schedule and pause or omit the schedule in dev.

19. A notebook hardcodes `prod_main.silver.orders`. Why is that bad for promotion?

    **Sample answer:** The same code cannot safely run in dev/test. Use a bundle variable or job parameter to supply the catalog/schema.

20. What should CI run before merging if it needs to catch invalid bundle YAML?

    **Sample answer:** `databricks bundle validate`, usually with the target flag such as `databricks bundle validate -t dev`.

21. What command deploys a bundle to prod?

    **Sample answer:** `databricks bundle deploy -t prod` or `databricks bundle deploy --target prod`.

22. What command runs a deployed bundle job named `orders_daily` in dev?

    **Sample answer:** `databricks bundle run orders_daily -t dev`.

23. What command shows bundle identity and deployed resources?

    **Sample answer:** `databricks bundle summary -t <target>`.

24. What is the difference between `bundle deploy` and `bundle run`?

    **Sample answer:** `deploy` creates or updates resources in the target workspace. `run` starts a deployed job, pipeline, or script.

25. A bundle deploy completed successfully, but the job did not process data. What might the developer have misunderstood?

    **Sample answer:** Deploying does not execute the job. They need to trigger the job separately, for example with `databricks bundle run` or a schedule.

26. A production job should run as a service principal, not the user who deployed it. Which bundle field controls this?

    **Sample answer:** `run_as`, usually with `service_principal_name` in the prod target.

27. Where can `run_as` be configured?

    **Sample answer:** At the top level, under a target, or on supported resources such as jobs.

28. Why use a service principal for production `run_as`?

    **Sample answer:** It decouples workflow execution from an individual user and allows more controlled production permissions.

29. A user leaves the company and production jobs stop because they ran as that user. What configuration would have reduced this risk?

    **Sample answer:** Configure production workflows to run as a service principal using `run_as`.

30. A CI/CD pipeline needs credentials to deploy to Databricks. Should the PAT or secret be committed in `databricks.yml`?

    **Sample answer:** No. Store credentials in the CI/CD secret store or use a supported identity method such as service principal/OIDC.

31. A developer runs `databricks bundle validate` with no `-t` flag. Which target is used?

    **Sample answer:** The default target, if one is configured.

32. Why should CI/CD usually pass `-t` or `--target` explicitly?

    **Sample answer:** To avoid accidentally validating or deploying the wrong environment.

33. A bundle has `dev` marked `default: true`. What command deploys dev without naming the target?

    **Sample answer:** `databricks bundle deploy`, though explicit `-t dev` is clearer in automation.

34. What is a Declarative Automation Bundle?

    **Sample answer:** A Databricks project definition as code, including source files, resource definitions, configuration, tests, and deployment targets.

35. What Databricks resources are commonly packaged in bundles for this exam domain?

    **Sample answer:** Lakeflow Jobs, Lakeflow Spark Declarative Pipelines, notebooks/source files, dashboards, permissions, and related configuration.

36. A Lakeflow Spark Declarative Pipeline must be promoted from test to prod. What should define it?

    **Sample answer:** A pipeline resource in the bundle, deployed to each target with `databricks bundle deploy -t <target>`.

37. A manual UI edit to a bundle-managed production job disappears after the next deploy. Why?

    **Sample answer:** The bundle is the source of truth; deploy reapplies the YAML-defined desired state.

38. How should a team change a production job schedule if the job is bundle-managed?

    **Sample answer:** Change the bundle YAML, review through Git/PR, then redeploy.

39. A bundle resource is removed from the YAML and the bundle is deployed. What can happen?

    **Sample answer:** If the resource was previously managed by that bundle deployment, it can be removed from the target workspace.

40. Which command is dangerous because it deletes deployed bundle resources?

    **Sample answer:** `databricks bundle destroy`.

41. A team wants a pre-merge check that validates syntax but does not deploy anything. Which command fits?

    **Sample answer:** `databricks bundle validate -t <target>`.

42. A team wants to preview deployment changes where supported. Which command may help?

    **Sample answer:** `databricks bundle plan -t <target>`.

43. What does `databricks bundle init` do?

    **Sample answer:** It creates a starter bundle project from a template.

44. A job should use `dev_main` in dev and `prod_main` in prod, but the notebook path should stay the same. What is the best design?

    **Sample answer:** Keep one notebook/source file and pass the catalog through a bundle variable or job parameter set by target.

45. What is the main risk of separate hand-created dev and prod jobs?

    **Sample answer:** Configuration drift. The jobs can silently diverge and no longer represent the same tested code.

46. A CI pipeline validates dev, deploys test, runs integration checks, waits for approval, then deploys prod. Is this a reasonable promotion flow?

    **Sample answer:** Yes. It promotes reviewed code through increasingly controlled environments.

47. A developer asks whether `databricks workspace import` is the preferred modern answer for deploying jobs and pipelines as code. What should you say?

    **Sample answer:** For bundle-based CI/CD scenarios, use Declarative Automation Bundles and bundle CLI commands. Workspace import is not the preferred answer for managing jobs and pipelines as code.

48. What identity is used to deploy a bundle versus run a workflow?

    **Sample answer:** The deploy identity is whoever or whatever authenticates the CLI. The run identity is controlled by the workflow or bundle `run_as` setting.

49. A prod target has a service principal in `run_as`, but that principal cannot read a Unity Catalog table. Will retries fix the job?

    **Sample answer:** No. Grant the required Unity Catalog privileges to the run identity.

50. In one sentence, what is the most important Domain 5 rule?

    **Sample answer:** Use Git branches and PRs for code changes, Declarative Automation Bundles with variables/targets for environment-specific deployment, and Databricks CLI commands to validate, deploy, and run workflows through CI/CD.
