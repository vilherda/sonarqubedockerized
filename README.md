# SonarQube on place

## Requirements

To have Docker installed

## Setup

1. create the following subdirectories in the directory that you have the file `docker-compose.yaml`: `project`, `sq_data`, `sq_extensions` and `sq_logs`
1. dump the source code of the project into subdirectory `project` directly, **NOT as a sub-subdirectory**
1. on a shell session located in the directory that you have the file `docker-compose.yaml` launch docker-compose process executing: `docker-compose up sq`
1. when you see the log message `"SonarQube is up"` on the shell you launch docker-compose command, open your browser at <http://localhost:9000>
1. click over `"log in"` button on the upper right corner of the page
1. login with credentials `admin`/`admin`
1. click over tab `"Administration"` tab
1. click over subtab `"Marketplace"`
1. scroll down and click over `"install"` button of next plugins to install them: `Git`, `SonarCSS`, `SonarHTML`, `SonarJS`, `SonarTS`, `SonarXML` and `YAML Analyzer` (consider to install other plugins if you want)
1. scroll up and you will see the button `"Restart Server"`: click on it. You will prompted to restart on a dialog box, click over `"Restart"` button
1. when you see the log message `"SonarQube is up"` on the shell session where you have launched docker-compose command, you will see the login page and you must to repeat the login process

## Use

On a new shell session located in the directory that you have the file `docker-compose.yaml` launch analysis process executing: `docker-compose up scanner` but consider to execute, **previously**, another processes like the 'testing' process or 'lint' process,... May be possible the results of this execution can be reused by Sonar Scanner.

When the process finishes, you can re-visit [your `SonarQube` service](http://localhost:9000).

## Disclaimers

Depending of the nature of your project and the tooling associated to its technology stack, may be possible you have to extend/modify some key files, lets say `package.json`. Let's explain this with an example.

If you add some _magical_ option to `lint` analysis (if you have it defined), you can _integrate_ the results. With next options:

> `-f json -o sq_lint_report.json`

you will generate a report in JSON format that can could be used by `SonarQube` analysis, so now you must add also an extra option on file `docker-compose.yaml`, change next line (the last line) from this:

> `command: sonar-scanner -Dsonar.host.url=http://sq:9000 -Dsonar.projectKey=parlacat -Dsonar.scm.provider=git`

to this:

> `command: sonar-scanner -Dsonar.host.url=http://sq:9000 -Dsonar.projectKey=parlacat -Dsonar.scm.provider=git` **`-Dsonar.eslint.reportPaths=./sq_lint_report.json`**

As you can see the report path is relative to project directory.

## Extra

- If may want to use [`Dependency-Check` plugin](https://github.com/dependency-check/dependency-check-sonar-plugin).
- To include information about test coverage/test execution see [documentation](https://docs.sonarqube.org/latest/analysis/coverage/).
