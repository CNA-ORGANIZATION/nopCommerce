# Workflow: Validate Project Files and Build Environment

This workflow step focuses on performing a pre-build check for common project and coding errors, such as duplicate project file entries, missing `using` directives, and validating the build environment.

## Prompt to Cline

```
"Perform the following validation steps:

1.  **Build Environment Validation:** Check for the existence of the `Microsoft.WebApplication.targets` file in the MSBuild directory. If the file is not found, inform the user that the build environment is missing the Web Development build tools and that the build is likely to fail. After completion, must validate that the file exists. If the file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_validate_build_environment_<datetimestamp>.md`.

2.  **Project File Validation:** Read all `.csproj` files in the solution and check for duplicate `<Compile>` entries for the same file. If duplicates are found, remove them. After completion, validate that there are no duplicate entries. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_validate_project_files_<datetimestamp>.md`.

3.  **Code File Validation:** Search all `.cs` files for instantiations of classes that are defined in other namespaces within the project. For each file where such an instantiation is found, verify that the corresponding `using` directive is present. If it is missing, add it. After completion, validate that all necessary `using` directives are present. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_validate_code_files_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that:
1.  The `Microsoft.WebApplication.targets` file exists in the MSBuild directory.
2.  There are no duplicate `<Compile>` entries in the `.csproj` files.
3.  All necessary `using` directives are present in the `.cs` files.

If any validation fails, Cline will log the failure in the corresponding log file as specified in the prompt.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
