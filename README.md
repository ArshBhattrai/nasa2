# Git Source Code Consolidator (PowerShell)

Author: Tech Nomad

Twitter: https://x.com/Tech_N0mad

This PowerShell script gathers source code files tracked by Git within a repository, filters out common non-source files (like binaries, images, dependencies, test files), and concatenates their paths and contents into a single output file (`output.txt` by default).

This is useful for creating a context package for code analysis, sharing relevant project files, or providing input to language models.

## Features

*   Uses `git ls-files` to reliably list files tracked by the current Git repository.
*   Applies a comprehensive set of filters to exclude common non-source code files and directories.
*   Sorts the list of included files for consistent output.
*   Generates a single output file (`output.txt`) containing:
    *   A header indicating the start of the file.
    *   A flat list of all included file paths.
    *   The full content of each included file, separated by the filename and `===`.
*   Provides progress indication using `Write-Progress` during file processing.
*   Includes basic error handling for missing Git executable, no matching files, and file read errors.

## Prerequisites

1.  **PowerShell:** The script is written for PowerShell (version 5.1 or later recommended, typically included with modern Windows; also available cross-platform).
2.  **Git:** Git must be installed and accessible from your system's PATH environment variable. The script relies on the `git ls-files` command.

## Usage

1.  **Save the script:** Save the PowerShell script code to a file, for example, `consolidate_code.ps1`, in the root directory of your Git repository.
2.  **Navigate to the repository:** Open PowerShell or Windows Terminal and change the directory (`cd`) to the root of your Git repository.
3.  **Run the script:** Execute the script using:
    ```powershell
    .\consolidate_code.ps1
    ```
4.  **Check the output:** A file named `output.txt` (by default) will be created in the same directory, containing the consolidated file list and contents.

## Filtering Logic

The script actively excludes files and directories based on the following patterns:

*   Files within any `tests/` directory at the start of the path (`^tests/`).
*   Common binary, compiled, or intermediate files (`.exe`, `.dll`, `.obj`, `.bin`, `.pdb`, `.cache`).
*   Log files (`.log`).
*   Markdown files (`.md`).
*   Image files (`.jpg`, `.jpeg`, `.png`, `.gif`, `.bmp`, `.ico`, `.svg`, `.webp`, `.tiff`).
*   Common dependency and build output directories (`node_modules/`, `packages/`, `dist/`, `build/`, `target/`).
*   IDE/Editor configuration directories (`.vs/`, `.vscode/`, `.idea/`).
*   Minified JavaScript or CSS files (`.min.js`, `.min.css`).
*   Git internal files/directories or macOS metadata (`.git`, `.DS_Store`).
*   Package lock files (`package-lock.json`, `yarn.lock`, `npm-shrinkwrap.json`).
*   .NET project, solution, and packaging files (`.csproj`, `.sln`, `.nuspec`, `.nupkg`).

## Output File Format (`output.txt`)

The generated `output.txt` file has the following structure:

```
--- START OF FILE output.txt ---

File list:
path/to/file1.ext
path/to/another/file2.ext
path/to/subdir/file3.ext
...

===

path/to/file1.ext

<Content of file1.ext>

===

path/to/another/file2.ext

<Content of file2.ext>

===

path/to/subdir/file3.ext

<Content of file3.ext>

...
<Content of last file>

```

## Customization

*   **Output File Path:** To change the name or location of the output file, modify the `$outputFile` variable at the beginning of the script.
    ```powershell
    $outputFile = "my_custom_output_name.txt"
    # or
    $outputFile = "C:\temp\project_dump.txt"
    ```
*   **Filtering Rules:** To adjust which files are included or excluded, modify the `-notmatch` conditions within the `Where-Object` block that filters the results of `git ls-files`. Add or remove conditions as needed based on regular expressions.

## License

This project is licensed under the MIT License - see the LICENSE.md file for details.
