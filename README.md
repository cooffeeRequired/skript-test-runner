# skript-test-runner
Skript test runner for addons

# Skript Test Runner

This tool is designed to automate the testing of Skript plugin versions with optional custom configurations and environments.

---

## 🚀 Features

- Detects your OS and chooses the correct shell and Gradle wrapper
- Downloads or detects a compatible JDK (Java 17)
- Clones or updates the Skript GitHub repository
- Allows multiple testing configurations
- Supports custom test scripts and additional plugins
- Interactive and non-interactive modes

---

## 🛠️ Requirements

- Python 3.7+
- Git installed and available in PATH
- Internet connection (for downloading JDK and Git repositories)
- A properly configured `config.json` file

---

## 📦 Installation

No installation is necessary. Simply place `test_runner.py` and `config.json` in the same directory and run the script.

---

## 🧠 How It Works
1. Loads configurations from config.json
2. Detects your operating system and sets the correct environment
3. Selects a configuration and a compatible JDK (Java 17)
4. Clones or updates the Skript repository
5. Optionally removes vanilla test files
6. Copies your custom test scripts
7. Injects extra plugins into Skript test environments
8. Sets the environment variables for Java
9. Executes the Gradle quickTest command
10. Outputs the test result and exit code

## 📁 Configuration File (`config.json`)

Here is an example structure:

```json
{
  "system": "auto",
  "configurations": [
    {
      "name": "Skript 2.9.2",
      "workspace_directory": ".",
      "test_script_directory": "./src/test/scripts/custom",
      "skript_repo_ref": "2.9.2",
      "run_vanilla_tests": false,
      "extra_plugins_directory": "./build/libs",
      "skript_repo_git_url": "https://github.com/SkriptLang/Skript.git",
      "jdk_path": "C:\\Users\\YourUser\\.jdks\\amazon-corretto-17"
    }
  ],
  "default_configuration": 0
}
```

## 🧪 How to Use
Run the script via command line:
`python test_runner.py [OPTIONS]`

### Options
*--configuration <int>	Index of the configuration to use (0-based) from the config file. If omitted and not in --no-interactive mode, the user will be prompted.
--jdk auto	Use an existing JDK installation (default).
--jdk download	Download and install JDK 17 automatically.
--system auto	Automatically detect the system (default).
--system windows	Force Windows settings.
--system wsl	Force WSL settings (useful on Windows Subsystem for Linux).
--system linux	Force Linux settings.
--system darwin	Force macOS settings.
--no-interactive	Run completely non-interactively. Required config must be provided via options or in config.json.*


## Example of [raw] usage
`python test_runner.py --configuration 0 --jdk auto --no-interactive`

## Example of gradle usage

**build.gradle.kts**
```kotlin
tasks.register("withTesting") {
    dependsOn("clean")
    dependsOn("shadowJar")
    doLast {
        println("> Task :running tests")
        exec {
            workingDir = projectDir
            commandLine("python", "test_runner.py", "--configuration=1", "--jdk=auto", "--system=auto", "--no-interactive")
        }
    }
}
```

**build.gradle**
```groovy
tasks.register('withTesting') {
    dependsOn 'clean'
    dependsOn 'shadowJar'
    doLast {
        println '> Task :running tests'
        exec {
            workingDir projectDir
            commandLine 'python', 'test_runner.py', '--configuration=1', '--jdk=auto', '--system=auto', '--no-interactive'
        }
    }
}
```
