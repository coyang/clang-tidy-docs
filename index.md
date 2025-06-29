# Clang-Tidy

# Clang-Tidy

`clang-tidy` is a clang-based C++ "linter" tool. Its purpose is to provide an
extensible framework for diagnosing and fixing typical programming errors, like
style violations, interface misuse, or bugs that can be deduced via static
analysis. `clang-tidy` is modular and provides a convenient interface for
writing new checks.

`clang-tidy` 是一个基于 clang 的 C++ "linter"工具。它旨在提供一个可扩展的框架，
用于诊断和修复常见的编程错误，如风格违规、接口误用或通过静态分析可以推断出的
bug。`clang-tidy` 采用模块化设计，并为编写新检查项提供了便捷的接口。

## Using Clang-Tidy

## 使用 Clang-Tidy

`clang-tidy` is a
[LibTooling](https://clang.llvm.org/docs/LibTooling.html)-based tool, and it's
easier to work with if you set up a compile command database for your project
(for an example of how to do this, see [How To Setup Tooling For
LLVM](https://clang.llvm.org/docs/HowToSetupToolingForLLVM.html)). You can also
specify compilation options on the command line after `--`:

`clang-tidy` 是一个基于[LibTooling](https://clang.llvm.org/docs/LibTooling.html)
的工具，如果你为项目设置了编译命令数据库，使用起来会更方便（如何设置可参考 [How
To Setup Tooling For
LLVM](https://clang.llvm.org/docs/HowToSetupToolingForLLVM.html)）。你也可以在命
令行的 `--` 后指定编译选项：

```console
$ clang-tidy test.cpp -- -Imy_project/include -DMY_DEFINES ...
```

If there are too many options or source files to specify on the command line,
you can store them in a parameter file, and use `clang-tidy`{.interpreted-text
role="program"} with that parameters file:

如果命令行中需要指定的选项或源文件太多，你可以将它们存储在参数文件中，然后用参数
文件调用 `clang-tidy`：

```console
$ clang-tidy @parameters_file
```

`clang-tidy` has its own checks and can also run Clang Static Analyzer checks.
Each check has a name and the checks to run can be chosen using the `-checks=`
option, which specifies a comma-separated list of positive and negative
(prefixed with `-`) globs. Positive globs add subsets of checks, and negative
globs remove them. For example,

`clang-tidy` 既有自己的检查项，也可以运行 Clang 静态分析器的检查项。每个检查项都
有一个名称，可以通过 `-checks=` 选项选择要运行的检查项，该选项接受以逗号分隔的正
向（添加，直接写名称）和负向（移除，以 `-`开头）通配符列表。例如：

```console
$ clang-tidy test.cpp -checks=-*,clang-analyzer-*,-clang-analyzer-cplusplus*
```

will disable all default checks (`-*`) and enable all `clang-analyzer-*` checks
except for `clang-analyzer-cplusplus*` ones.

这条命令会禁用所有默认检查（`-*`），只启用所有 `clang-analyzer-*` 检查项，但排除
`clang-analyzer-cplusplus*`。

The `-list-checks` option lists all the enabled checks. When used without
`-checks=`, it shows checks enabled by default. Use `-checks=*` to see all
available checks or with any other value of `-checks=` to see which checks are
enabled by this value.

`-list-checks` 选项会列出所有已启用的检查项。如果不带 `-checks=` 使用，则显示默
认启用的检查项。用 `-checks=*` 可以查看所有可用检查项，或与其他 `-checks=` 组合
查看当前配置下启用的检查项。

目前有以下几组检查项：

| 名称前缀             | 英文说明                                                                                        | 中文说明                                               |
| -------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `abseil-`            | Checks related to Abseil library.                                                               | 与 Abseil 库相关的检查。                               |
| `altera-`            | Checks related to OpenCL programming for FPGAs.                                                 | 与 FPGA 的 OpenCL 编程相关的检查。                     |
| `android-`           | Checks related to Android.                                                                      | 与 Android 相关的检查。                                |
| `boost-`             | Checks related to Boost library.                                                                | 与 Boost 库相关的检查。                                |
| `bugprone-`          | Checks that target bug-prone code constructs.                                                   | 针对易出 bug 代码结构的检查。                          |
| `cert-`              | Checks related to CERT Secure Coding Guidelines.                                                | 与 CERT 安全编码规范相关的检查。                       |
| `clang-analyzer-`    | Clang Static Analyzer checks.                                                                   | Clang 静态分析器的检查。                               |
| `concurrency-`       | Checks related to concurrent programming (including threads, fibers, coroutines, etc.).         | 与并发编程相关的检查（包括线程、纤程、协程等）。       |
| `cppcoreguidelines-` | Checks related to C++ Core Guidelines.                                                          | 与 C++ 核心准则相关的检查。                            |
| `darwin-`            | Checks related to Darwin coding conventions.                                                    | 与 Darwin 编码规范相关的检查。                         |
| `fuchsia-`           | Checks related to Fuchsia coding conventions.                                                   | 与 Fuchsia 编码规范相关的检查。                        |
| `google-`            | Checks related to Google coding conventions.                                                    | 与 Google 编码规范相关的检查。                         |
| `hicpp-`             | Checks related to High Integrity C++ Coding Standard.                                           | 与高完整性 C++ 编码标准相关的检查。                    |
| `linuxkernel-`       | Checks related to the Linux Kernel coding conventions.                                          | 与 Linux 内核编码规范相关的检查。                      |
| `llvm-`              | Checks related to the LLVM coding conventions.                                                  | 与 LLVM 编码规范相关的检查。                           |
| `llvmlibc-`          | Checks related to the LLVM-libc coding standards.                                               | 与 LLVM-libc 编码标准相关的检查。                      |
| `misc-`              | Checks that we didn't have a better category for.                                               | 其他未归类的检查。                                     |
| `modernize-`         | Checks that advocate usage of modern (currently "modern" means "C++11") language constructs.    | 鼓励使用现代（目前指 C++11）语言特性的检查。           |
| `mpi-`               | Checks related to MPI (Message Passing Interface).                                              | 与 MPI（消息传递接口）相关的检查。                     |
| `objc-`              | Checks related to Objective-C coding conventions.                                               | 与 Objective-C 编码规范相关的检查。                    |
| `openmp-`            | Checks related to OpenMP API.                                                                   | 与 OpenMP API 相关的检查。                             |
| `performance-`       | Checks that target performance-related issues.                                                  | 针对性能相关问题的检查。                               |
| `portability-`       | Checks that target portability-related issues that don't relate to any particular coding style. | 针对与可移植性相关但不特定于某种编码风格的问题的检查。 |
| `readability-`       | Checks that target readability-related issues that don't relate to any particular coding style. | 针对与可读性相关但不特定于某种编码风格的问题的检查。   |
| `zircon-`            | Checks related to Zircon kernel coding conventions.                                             | 与 Zircon 内核编码规范相关的检查。                     |

Clang diagnostics are treated in a similar way as check diagnostics. Clang
diagnostics are displayed by `clang-tidy` and can be filtered out using the
`-checks=` option. However, the `-checks=` option does not affect compilation
arguments, so it cannot turn on Clang warnings which are not already turned on
in the build configuration. The `-warnings-as-errors=` option upgrades any
warnings emitted under the `-checks=` flag to errors (but it does not enable any
checks itself).

Clang 诊断与检查项诊断的处理方式类似。Clang 诊断会由`clang-tidy` 显示，并可通过
`-checks=` 选项进行过滤。然而，`-checks=` 选项不会影响编译参数，因此无法启用构建
配置中未开启的 Clang 警告。`-warnings-as-errors=` 选项会将 `-checks=` 标志下产生
的所有警告升级为错误（但不会启用任何检查项）。

Clang diagnostics have check names starting with `clang-diagnostic-`.
Diagnostics which have a corresponding warning option, are named
`clang-diagnostic-<warning-option>`, e.g. Clang warning controlled by
`-Wliteral-conversion` will be reported with check name
`clang-diagnostic-literal-conversion`.

Clang 诊断的检查项名称以 `clang-diagnostic-` 开头。具有对应警告选项的诊断项命名
为 `clang-diagnostic-<warning-option>`，例如由 `-Wliteral-conversion` 控制的
Clang 警告会以 `clang-diagnostic-literal-conversion` 作为检查项名称报告。

The `-fix` flag instructs `clang-tidy` to fix found errors if supported by
corresponding checks.

`-fix` 标志会指示 `clang-tidy` 在对应检查项支持的情况下自动修复发现的错误。

An overview of all the command-line options:

所有命令行选项概览：

```console
$ clang-tidy --help
USAGE: clang-tidy [options] <source0> [... <sourceN>]

OPTIONS:

Generic Options:

  --help                           - Display available options (--help-hidden for more)
  --help-list                      - Display list of available options (--help-list-hidden for more)
  --version                        - Display the version of this program

clang-tidy options:

  --checks=<string>                - Comma-separated list of globs with optional '-'
                                     prefix. Globs are processed in order of
                                     appearance in the list. Globs without '-'
                                     prefix add checks with matching names to the
                                     set, globs with the '-' prefix remove checks
                                     with matching names from the set of enabled
                                     checks. This option's value is appended to the
                                     value of the 'Checks' option in .clang-tidy
                                     file, if any.
  --config=<string>                - Specifies a configuration in YAML/JSON format:
                                       -config="{Checks: '*',
                                                 CheckOptions: {x: y}}"
                                     When the value is empty, clang-tidy will
                                     attempt to find a file named .clang-tidy for
                                     each source file in its parent directories.
  --config-file=<string>           - Specify the path of .clang-tidy or custom config file:
                                      e.g. --config-file=/some/path/myTidyConfigFile
                                     This option internally works exactly the same way as
                                      --config option after reading specified config file.
                                     Use either --config-file or --config, not both.
  --dump-config                    - Dumps configuration in the YAML format to
                                     stdout. This option can be used along with a
                                     file name (and '--' if the file is outside of a
                                     project with configured compilation database).
                                     The configuration used for this file will be
                                     printed.
                                     Use along with -checks=* to include
                                     configuration of all checks.
  --enable-check-profile           - Enable per-check timing profiles, and print a
                                     report to stderr.
  --enable-module-headers-parsing  - Enables preprocessor-level module header parsing
                                     for C++20 and above, empowering specific checks
                                     to detect macro definitions within modules. This
                                     feature may cause performance and parsing issues
                                     and is therefore considered experimental.
  --exclude-header-filter=<string> - Regular expression matching the names of the
                                     headers to exclude diagnostics from. Diagnostics
                                     from the main file of each translation unit are
                                     always displayed.
                                     Must be used together with --header-filter.
                                     Can be used together with -line-filter.
                                     This option overrides the 'ExcludeHeaderFilterRegex'
                                     option in .clang-tidy file, if any.
  --explain-config                 - For each enabled check explains, where it is
                                     enabled, i.e. in clang-tidy binary, command
                                     line or a specific configuration file.
  --export-fixes=<filename>        - YAML file to store suggested fixes in. The
                                     stored fixes can be applied to the input source
                                     code with clang-apply-replacements.
  --extra-arg=<string>             - Additional argument to append to the compiler command line
  --extra-arg-before=<string>      - Additional argument to prepend to the compiler command line
  --fix                            - Apply suggested fixes. Without -fix-errors
                                     clang-tidy will bail out if any compilation
                                     errors were found.
  --fix-errors                     - Apply suggested fixes even if compilation
                                     errors were found. If compiler errors have
                                     attached fix-its, clang-tidy will apply them as
                                     well.
  --fix-notes                      - If a warning has no fix, but a single fix can
                                     be found through an associated diagnostic note,
                                     apply the fix.
                                     Specifying this flag will implicitly enable the
                                     '--fix' flag.
  --format-style=<string>          - Style for formatting code around applied fixes:
                                       - 'none' (default) turns off formatting
                                       - 'file' (literally 'file', not a placeholder)
                                         uses .clang-format file in the closest parent
                                         directory
                                       - '{ <json> }' specifies options inline, e.g.
                                         -format-style='{BasedOnStyle: llvm, IndentWidth: 8}'
                                       - 'llvm', 'google', 'webkit', 'mozilla'
                                     See clang-format documentation for the up-to-date
                                     information about formatting styles and options.
                                     This option overrides the 'FormatStyle` option in
                                     .clang-tidy file, if any.
  --header-filter=<string>         - Regular expression matching the names of the
                                     headers to output diagnostics from. Diagnostics
                                     from the main file of each translation unit are
                                     always displayed.
                                     Can be used together with -line-filter.
                                     This option overrides the 'HeaderFilterRegex'
                                     option in .clang-tidy file, if any.
  --line-filter=<string>           - List of files with line ranges to filter the
                                     warnings. Can be used together with
                                     -header-filter. The format of the list is a
                                     JSON array of objects:
                                       [
                                         {"name":"file1.cpp","lines":[[1,3],[5,7]]},
                                         {"name":"file2.h"}
                                       ]
  --list-checks                    - List all enabled checks and exit. Use with
                                     -checks=* to list all available checks.
  --load=<pluginfilename>          - Load the specified plugin
  -p <string>                      - Build path
  --quiet                          - Run clang-tidy in quiet mode. This suppresses
                                     printing statistics about ignored warnings and
                                     warnings treated as errors if the respective
                                     options are specified.
  --store-check-profile=<prefix>   - By default reports are printed in tabulated
                                     format to stderr. When this option is passed,
                                     these per-TU profiles are instead stored as JSON.
  --system-headers                 - Display the errors from system headers.
                                     This option overrides the 'SystemHeaders' option
                                     in .clang-tidy file, if any.
  --use-color                      - Use colors in diagnostics. If not set, colors
                                     will be used if the terminal connected to
                                     standard output supports colors.
                                     This option overrides the 'UseColor' option in
                                     .clang-tidy file, if any.
  --verify-config                  - Check the config files to ensure each check and
                                     option is recognized.
  --vfsoverlay=<filename>          - Overlay the virtual filesystem described by file
                                     over the real file system.
  --warnings-as-errors=<string>    - Upgrades warnings to errors. Same format as
                                     '-checks'.
                                     This option's value is appended to the value of
                                     the 'WarningsAsErrors' option in .clang-tidy
                                     file, if any.
  --allow-no-checks                - Allow empty enabled checks. This suppresses
                                     the "no checks enabled" error when disabling
                                     all of the checks.

-p <build-path> is used to read a compile command database.

-p <build-path> 用于读取编译命令数据库。

  For example, it can be a CMake build directory in which a file named
  compile_commands.json exists (use -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
  CMake option to get this output). When no build path is specified,
  a search for compile_commands.json will be attempted through all
  parent paths of the first input file . See:
  https://clang.llvm.org/docs/HowToSetupToolingForLLVM.html for an
  example of setting up Clang Tooling on a source tree.

  例如，它可以是一个 CMake 构建目录，其中存在名为 compile_commands.json 的文件（可通过 CMake 选项 -DCMAKE_EXPORT_COMPILE_COMMANDS=ON 生成）。如果未指定构建路径，将会在第一个输入文件的所有父路径中查找 compile_commands.json。参见：https://clang.llvm.org/docs/HowToSetupToolingForLLVM.html 获取在源码树上设置 Clang Tooling 的示例。

<source0> ... specify the paths of source files. These paths are looked up in the compile command database. If the path of a file is absolute, it needs to point into CMake's source tree. If the path is relative, the current working directory needs to be in the CMake source tree and the file must be in a subdirectory of the current working directory. "./" prefixes in the relative files will be automatically removed, but the rest of a relative path must be a suffix of a path in the compile command database.

<source0> ... 指定源文件路径。这些路径会在编译命令数据库中查找。如果文件路径是绝对路径，则必须指向 CMake 源码树。如果是相对路径，则当前工作目录必须在 CMake 源码树中，且文件必须位于当前工作目录的子目录下。相对路径中的 "./" 前缀会被自动移除，但剩余部分必须是编译命令数据库中某个路径的后缀。

Parameters files:
  A large number of options or source files can be passed as parameter files by use '@parameter-file' in the command line.

参数文件：
  如果需要传递大量选项或源文件，可以通过在命令行中使用 '@parameter-file' 的方式传递参数文件。

Configuration files:
  clang-tidy attempts to read configuration for each source file from a .clang-tidy file located in the closest parent directory of the source file. The .clang-tidy file is specified in YAML format. If any configuration options have a corresponding command-line option, command-line option takes precedence.

配置文件：
  clang-tidy 会尝试从每个源文件最近的父目录中的 .clang-tidy 文件读取配置。.clang-tidy 文件采用 YAML 格式。如果某些配置项有对应的命令行选项，则命令行选项优先生效。

  The following configuration options may be used in a .clang-tidy file:

  .clang-tidy 文件中可用的配置项如下：

  CheckOptions                 - List of key-value pairs defining check-specific options. Example:
                                   CheckOptions:
                                     some-check.SomeOption: 'some value'
  Checks                       - Same as '--checks'. Additionally, the list of globs can be specified as a list instead of a string.
  ExcludeHeaderFilterRegex     - Same as '--exclude-header-filter'.
  ExtraArgs                    - Same as '--extra-arg'.
  ExtraArgsBefore              - Same as '--extra-arg-before'.
  FormatStyle                  - Same as '--format-style'.
  HeaderFileExtensions         - File extensions to consider to determine if a given diagnostic is located in a header file.
  HeaderFilterRegex            - Same as '--header-filter'.
  ImplementationFileExtensions - File extensions to consider to determine if a given diagnostic is located in an implementation file.
  InheritParentConfig          - If this option is true in a config file, the configuration file in the parent directory (if any exists) will be taken and the current config file will be applied on top of the parent one.
  SystemHeaders                - Same as '--system-headers'.
  UseColor                     - Same as '--use-color'.
  User                         - Specifies the name or e-mail of the user running clang-tidy. This option is used, for example, to place the correct user name in TODO() comments in the relevant check.
  WarningsAsErrors             - Same as '--warnings-as-errors'.

  CheckOptions                 - 定义检查项专用选项的键值对列表。例如：
                                   CheckOptions:
                                     some-check.SomeOption: 'some value'
  Checks                       - 同 '--checks'。此外，通配符列表也可以用列表而非字符串形式指定。
  ExcludeHeaderFilterRegex     - 同 '--exclude-header-filter'。
  ExtraArgs                    - 同 '--extra-arg'。
  ExtraArgsBefore              - 同 '--extra-arg-before'。
  FormatStyle                  - 同 '--format-style'。
  HeaderFileExtensions         - 用于判断诊断是否位于头文件的文件扩展名。
  HeaderFilterRegex            - 同 '--header-filter'。
  ImplementationFileExtensions - 用于判断诊断是否位于实现文件的文件扩展名。
  InheritParentConfig          - 如果该选项在配置文件中为 true，则会读取父目录（如有）中的配置文件，并在其基础上应用当前配置文件。
  SystemHeaders                - 同 '--system-headers'。
  UseColor                     - 同 '--use-color'。
  User                         - 指定运行 clang-tidy 的用户名称或邮箱。例如用于在相关检查项的 TODO() 注释中插入正确的用户名。
  WarningsAsErrors             - 同 '--warnings-as-errors'。

  The effective configuration can be inspected using --dump-config:

  可通过 --dump-config 查看实际生效的配置：

    $ clang-tidy --dump-config
    ---
    Checks:              '-*,some-check'
    WarningsAsErrors:    ''
    HeaderFileExtensions:         ['', 'h','hh','hpp','hxx']
    ImplementationFileExtensions: ['c','cc','cpp','cxx']
    HeaderFilterRegex:   ''
    FormatStyle:         none
    InheritParentConfig: true
    User:                user
    CheckOptions:
      some-check.SomeOption: 'some value'
    ...

```

## Suppressing Undesired Diagnostics {#clang-tidy-nolint}

## 抑制不需要的诊断 {#clang-tidy-nolint}

`clang-tidy` diagnostics are intended to call out code that does not adhere to a coding standard, or is otherwise problematic in some way. However, if the code is known to be correct, it may be useful to silence the warning. Some clang-tidy checks provide a check-specific way to silence the diagnostics, e.g. [bugprone-use-after-move](checks/bugprone/use-after-move.html) can be silenced by re-initializing the variable after it has been moved out, [bugprone-string-integer-assignment](checks/bugprone/string-integer-assignment.html) can be suppressed by explicitly casting the integer to `char`, [readability-implicit-bool-conversion](checks/readability/implicit-bool-conversion.html) can also be suppressed by using explicit casts, etc.

`clang-tidy` 的诊断旨在指出不符合编码规范或存在其他问题的代码。但如果代码已知是正确的，可能希望屏蔽这些警告。一些 clang-tidy 检查项提供了专用的抑制方式，例如 [bugprone-use-after-move](checks/bugprone/use-after-move.html) 可以通过在变量被移动后重新初始化来抑制，[bugprone-string-integer-assignment](checks/bugprone/string-integer-assignment.html) 可以通过显式将整数转换为 `char` 抑制，[readability-implicit-bool-conversion](checks/readability/implicit-bool-conversion.html) 也可以通过显式类型转换来抑制，等等。

If a specific suppression mechanism is not available for a certain warning, or its use is not desired for some reason, `clang-tidy` has a generic mechanism to suppress diagnostics using `NOLINT`, `NOLINTNEXTLINE`, and `NOLINTBEGIN` ... `NOLINTEND` comments.

如果某个警告没有专用的抑制机制，或者不希望使用该机制，`clang-tidy` 提供了通用的注释方式来抑制诊断：`NOLINT`、`NOLINTNEXTLINE` 以及 `NOLINTBEGIN` ... `NOLINTEND`。

The `NOLINT` comment instructs `clang-tidy` to ignore warnings on the _same line_ (it doesn't apply to a function, a block of code or any other language construct; it applies to the line of code it is on). If introducing the comment on the same line would change the formatting in an undesired way, the `NOLINTNEXTLINE` comment allows suppressing clang-tidy warnings on the _next line_. The `NOLINTBEGIN` and `NOLINTEND` comments allow suppressing clang-tidy warnings on _multiple lines_ (affecting all lines between the two comments).

`NOLINT` 注释会让 `clang-tidy` 忽略同一行上的警告（它不作用于函数、代码块或其他语言结构，仅作用于所在行）。如果在同一行添加注释会影响格式，可以用 `NOLINTNEXTLINE` 注释来抑制 _下一行_ 的警告。`NOLINTBEGIN` 和 `NOLINTEND` 注释可以抑制 _多行_ 的警告（作用于两者之间的所有行）。

All comments can be followed by an optional list of check names in parentheses (see below for the formal syntax). The list of check names supports globbing, with the same format and semantics as for enabling checks. Note: negative globs are ignored here, as they would effectively re-activate the warning.

所有注释后都可以加括号，列出要抑制的检查项名称（见下文的正式语法）。检查项名称支持通配符，格式和语义与启用检查项时相同。注意：此处忽略负向通配符，因为它们会重新激活警告。

For example:

例如：

```c++
class Foo {
  // Suppress all the diagnostics for the line
  Foo(int param); // NOLINT

  // Consider explaining the motivation to suppress the warning
  Foo(char param); // NOLINT: Allow implicit conversion from `char`, because <some valid reason>

  // Silence only the specified checks for the line
  Foo(double param); // NOLINT(google-explicit-constructor, google-runtime-int)

  // Silence all checks from the `google` module
  Foo(bool param); // NOLINT(google*)

  // Silence all checks ending with `-avoid-c-arrays`
  int array[10]; // NOLINT(*-avoid-c-arrays)

  // Silence only the specified diagnostics for the next line
  // NOLINTNEXTLINE(google-explicit-constructor, google-runtime-int)
  Foo(bool param);

  // Silence all checks from the `google` module for the next line
  // NOLINTNEXTLINE(google*)
  Foo(bool param);

  // Silence all checks ending with `-avoid-c-arrays` for the next line
  // NOLINTNEXTLINE(*-avoid-c-arrays)
  int array[10];

  // Silence only the specified checks for all lines between the BEGIN and END
  // NOLINTBEGIN(google-explicit-constructor, google-runtime-int)
  Foo(short param);
  Foo(long param);
  // NOLINTEND(google-explicit-constructor, google-runtime-int)

  // Silence all checks from the `google` module for all lines between the BEGIN and END
  // NOLINTBEGIN(google*)
  Foo(bool param);
  // NOLINTEND(google*)

  // Silence all checks ending with `-avoid-c-arrays` for all lines between the BEGIN and END
  // NOLINTBEGIN(*-avoid-c-arrays)
  int array[10];
  // NOLINTEND(*-avoid-c-arrays)
};
````

```c++
class Foo {
  // 抑制本行所有诊断
  Foo(int param); // NOLINT

  // 建议说明抑制警告的原因
  Foo(char param); // NOLINT: 允许从 `char` 隐式转换，因为 <某个合理原因>

  // 仅抑制本行指定的检查项
  Foo(double param); // NOLINT(google-explicit-constructor, google-runtime-int)

  // 抑制 `google` 模块下所有检查项
  Foo(bool param); // NOLINT(google*)

  // 抑制所有以 `-avoid-c-arrays` 结尾的检查项
  int array[10]; // NOLINT(*-avoid-c-arrays)

  // 仅抑制下一行指定的诊断
  // NOLINTNEXTLINE(google-explicit-constructor, google-runtime-int)
  Foo(bool param);

  // 抑制下一行 `google` 模块下所有检查项
  // NOLINTNEXTLINE(google*)
  Foo(bool param);

  // 抑制下一行所有以 `-avoid-c-arrays` 结尾的检查项
  // NOLINTNEXTLINE(*-avoid-c-arrays)
  int array[10];

  // 抑制 BEGIN 和 END 之间所有行的指定检查项
  // NOLINTBEGIN(google-explicit-constructor, google-runtime-int)
  Foo(short param);
  Foo(long param);
  // NOLINTEND(google-explicit-constructor, google-runtime-int)

  // 抑制 BEGIN 和 END 之间所有行的 `google` 模块检查项
  // NOLINTBEGIN(google*)
  Foo(bool param);
  // NOLINTEND(google*)

  // 抑制 BEGIN 和 END 之间所有行的以 `-avoid-c-arrays` 结尾的检查项
  // NOLINTBEGIN(*-avoid-c-arrays)
  int array[10];
  // NOLINTEND(*-avoid-c-arrays)
};
```

The formal syntax of `NOLINT`, `NOLINTNEXTLINE`, and `NOLINTBEGIN` ...
`NOLINTEND` is the following:

`NOLINT`、`NOLINTNEXTLINE` 以及 `NOLINTBEGIN` ... `NOLINTEND` 的正式语法如下：

```
lint-comment:
  lint-command
  lint-command lint-args

lint-args:
  ( check-name-list )

check-name-list:
  check-name
  check-name-list , check-name

lint-command:
  NOLINT
  NOLINTNEXTLINE
  NOLINTBEGIN
  NOLINTEND
```

Note that whitespaces between
`NOLINT`/`NOLINTNEXTLINE`/`NOLINTBEGIN`/`NOLINTEND` and the opening parenthesis
are not allowed (in this case the comment will be treated just as
`NOLINT`/`NOLINTNEXTLINE`/`NOLINTBEGIN`/`NOLINTEND`), whereas in the check names
list (inside the parentheses), whitespaces can be used and will be ignored.

注意：`NOLINT`/`NOLINTNEXTLINE`/`NOLINTBEGIN`/`NOLINTEND` 与左括号之间不能有空格
（否则注释只会被视为 `NOLINT`/`NOLINTNEXTLINE`/`NOLINTBEGIN`/`NOLINTEND`），而括
号内的检查项名称列表可以有空格，且会被忽略。

All `NOLINTBEGIN` comments must be paired by an equal number of `NOLINTEND`
comments. Moreover, a pair of comments must have matching arguments -- for
example, `NOLINTBEGIN(check-name)` can be paired with `NOLINTEND(check-name)`
but not with `NOLINTEND` [(zero arguments)]{.title-ref}. `clang-tidy` will
generate a `clang-tidy-nolint` error diagnostic if any `NOLINTBEGIN`/`NOLINTEND`
comment violates these requirements.

所有 `NOLINTBEGIN` 注释都必须与同数量的 `NOLINTEND` 注释配对。此外，配对的注释参
数必须一致——例如，`NOLINTBEGIN(check-name)` 必须与 `NOLINTEND(check-name)` 配
对，不能与 `NOLINTEND`（无参数）配对。如果有任何 `NOLINTBEGIN`/`NOLINTEND` 注释
违反这些要求，`clang-tidy` 会生成`clang-tidy-nolint` 错误诊断。
