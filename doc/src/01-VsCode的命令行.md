# vscode查看已安装的插件列表
`code --list-extensions`

# `code -h`查看全部帮助
```
Visual Studio Code 1.77.0

Usage: code [options][paths...]

To read from stdin, append '-' (e.g. 'ps aux | grep code | code -')

Options
  -d --diff <file> <file>                    Compare two files with each
                                             other.
  -m --merge <path1> <path2> <base> <result> Perform a three-way merge by
                                             providing paths for two modified
                                             versions of a file, the common
                                             origin of both modified versions
                                             and the output file to save merge
                                             results.
  -a --add <folder>                          Add folder(s) to the last active
                                             window.
  -g --goto <file:line[:character]>          Open a file at the path on the
                                             specified line and character
                                             position.
  -n --new-window                            Force to open a new window.
  -r --reuse-window                          Force to open a file or folder in
                                             an already opened window.
  -w --wait                                  Wait for the files to be closed
                                             before returning.
  --locale <locale>                          The locale to use (e.g. en-US or
                                             zh-TW).
  --user-data-dir <dir>                      Specifies the directory that user
                                             data is kept in. Can be used to
                                             open multiple distinct instances
                                             of Code.
  --profile <profileName>                    Opens the provided folder or
                                             workspace with the given profile
                                             and associates the profile with
                                             the workspace. If the profile
                                             does not exist, a new empty one
                                             is created. A folder or workspace
                                             must be provided for the profile
                                             to take effect.
  -h --help                                  Print usage.

Extensions Management
  --extensions-dir <dir>              Set the root path for extensions.
  --list-extensions                   List the installed extensions.
  --show-versions                     Show versions of installed extensions,
                                      when using --list-extensions.
  --category <category>               Filters installed extensions by provided
                                      category, when using --list-extensions.
  --install-extension <ext-id | path> Installs or updates an extension. The
                                      argument is either an extension id or a
                                      path to a VSIX. The identifier of an
                                      extension is '${publisher}.${name}'. Use
                                      '--force' argument to update to latest
                                      version. To install a specific version
                                      provide '@${version}'. For example:
                                      'vscode.csharp@1.2.3'.
  --pre-release                       Installs the pre-release version of the
                                      extension, when using
                                      --install-extension
  --uninstall-extension <ext-id>      Uninstalls an extension.
  --enable-proposed-api <ext-id>      Enables proposed API features for
                                      extensions. Can receive one or more
                                      extension IDs to enable individually.

Troubleshooting
  -v --version                    Print version.
  --verbose                       Print verbose output (implies --wait).
  --log <level>                   Log level to use. Default is 'info'. Allowed
                                  values are 'critical', 'error', 'warn',
                                  'info', 'debug', 'trace', 'off'. You can
                                  also configure the log level of an extension
                                  by passing extension id and log level in the
                                  following format:
                                  '${publisher}.${name}:${logLevel}'. For
                                  example: 'vscode.csharp:trace'. Can receive
                                  one or more such entries.
  -s --status                     Print process usage and diagnostics
                                  information.
  --prof-startup                  Run CPU profiler during startup.
  --disable-extensions            Disable all installed extensions. This option
                                  is not persisted and is effective only when
                                  the command opens a new window.
  --disable-extension <ext-id>    Disable the provided extension. This option
                                  is not persisted and is effective only when
                                  the command opens a new window.
  --sync <on | off>               Turn sync on or off.
  --inspect-extensions <port>     Allow debugging and profiling of extensions.
                                  Check the developer tools for the connection
                                  URI.
  --inspect-brk-extensions <port> Allow debugging and profiling of extensions
                                  with the extension host being paused after
                                  start. Check the developer tools for the
                                  connection URI.
  --disable-gpu                   Disable GPU hardware acceleration.
  --max-memory <memory>           Max memory size for a window (in Mbytes).
  --telemetry                     Shows all telemetry events which VS code
                                  collects.

Subcommands
  tunnel       Make the current machine accessible from vscode.dev or other
               machines through a secure tunnel
```
