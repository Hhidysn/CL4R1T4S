{functions}
{
  "description": "为 Web 服务器启动浏览器预览。这允许用户正常与 Web 服务器交互，并将来自 Web 服务器的控制台日志和其他信息提供给 Cascade。请注意，此工具调用不会自动为用户打开浏览器预览，他们必须点击所提供的按钮之一才能在浏览器中将其打开。",
  "name": "browser_preview",
  "parameters": {
    "properties": {
      "Name": {
        "description": "目标 Web 服务器的 3-5 个单词的简短名称。应采用首字母大写格式，例如 'Personal Website'。格式化为简单字符串，而不是 markdown；请直接输出标题，不要带有 'Title:' 或任何类似前缀。",
        "type": "string"
      },
      "Url": {
        "description": "要为其提供浏览器预览的目标 Web 服务器的 URL。应包含协议方案（例如 http:// 或 https://）、域名（例如 localhost 或 127.0.0.1）和端口（例如 :8080），但不包含路径。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 Web 应用程序的 windsurf_deployment_id 检查部署状态，并确定应用程序构建是否已成功以及是否已被认领。除非用户要求，否则请勿运行此项。它只能在 deploy_web_app 工具调用之后运行。",
  "name": "check_deploy_status",
  "parameters": {
    "properties": {
      "WindsurfDeploymentId": {
        "description": "我们要检查其状态的部署对应的 Windsurf 部署 ID。这不是 project_id。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "从代码库中查找与搜索查询最相关的代码片段。当搜索查询更精确且与代码的功能或用途相关时，此功能效果最佳。如果提出非常宽泛的问题（例如询问大型组件或系统的通用 '框架' 或 '实现'），结果将会很差。将仅显示靠前项的完整代码内容，且它们也可能会被截断。对于其他项，它将仅显示文档字符串和签名。使用具有相同路径和节点名称的 view_code_item 可以查看任何项目的完整代码内容。请注意，如果你尝试在超过 500 个文件上进行搜索，搜索结果的质量将显著变差。尽量仅在确实必要时才在大量文件上进行搜索。",
  "name": "codebase_search",
  "parameters": {
    "properties": {
      "Query": {
        "description": "搜索查询",
        "type": "string"
      },
      "TargetDirectories": {
        "description": "要在其中进行搜索的目录的绝对路径列表",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "通过先前执行的终端命令的 ID 获取其状态。返回当前状态（运行中、已完成）、由输出优先级指定的输出行，以及任何错误（如果存在）。请勿尝试检查后台命令 ID 以外的任何 ID 的状态。",
  "name": "command_status",
  "parameters": {
    "properties": {
      "CommandId": {
        "description": "要获取其状态的命令的 ID",
        "type": "string"
      },
      "OutputCharacterCount": {
        "description": "要查看的字符数。尽可能将此值设小，以避免过多的内存使用。",
        "type": "integer"
      },
      "OutputPriority": {
        "description": "显示命令输出的优先级。必须是以下之一：'top'（显示最早的行）、'bottom'（显示最新的行）或 'split'（优先显示最早和最新的行，排除中间部分）",
        "enum": ["top", "bottom", "split"],
        "type": "string"
      },
      "WaitDurationSeconds": {
        "description": "在获取状态之前等待命令完成的秒数。如果命令在此持续时间之前完成，此工具调用将提前返回。设为 0 可立即获取命令的状态。如果你只对等待命令完成感兴趣，请设为 60。",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "将与用户及其任务相关的重要上下文保存到记忆数据库中。\n要保存的上下文示例：\n- 用户偏好\n- 用户明确要求记住某些内容或以其他方式改变你的行为\n- 重要的代码片段\n- 技术栈\n- 项目结构\n- 重大里程碑或功能\n- 新的设计模式和架构决策\n- 你认为需要记住的任何其他信息。\n在创建新记忆之前，首先检查数据库中是否已存在语义相关的记忆。如果找到，请对其进行更新而不是创建副本。\n必要时使用此工具删除不正确的记忆。",
  "name": "create_memory",
  "parameters": {
    "properties": {
      "Action": {
        "description": "对 MEMORY 采取的操作类型。必须是 'create'、'update' 或 'delete' 之一",
        "enum": ["create", "update", "delete"],
        "type": "string"
      },
      "Content": {
        "description": "新 MEMORY 或更新后的 MEMORY 的内容。删除现有 MEMORY 时，请将此项留空。",
        "type": "string"
      },
      "CorpusNames": {
        "description": "与 MEMORY 关联的工作区的 CorpusNames。每个元素必须与你的系统提示中提供的 CorpusNames 之一完全匹配（包括所有符号）。仅在创建新 MEMORY 时使用。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Id": {
        "description": "要更新或删除的现有 MEMORY 的 Id。创建新 MEMORY 时，请将此项留空。",
        "type": "string"
      },
      "Tags": {
        "description": "与 MEMORY 关联的标签。这些将用于过滤或检索 MEMORY。仅在创建新 MEMORY 时使用。使用 snake_case。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Title": {
        "description": "新 MEMORY 或更新后的 MEMORY 的描述性标题。创建或更新记忆时，此项是必需的。删除现有 MEMORY 时，请将此项留空。",
        "type": "string"
      },
      "UserTriggered": {
        "description": "如果用户明确要求你创建/修改此记忆，请设为 true。",
        "type": "boolean"
      }
    },
    "type": "object"
  }
}

{
  "description": "将 JavaScript Web 应用程序部署到像 Netlify 这样的部署提供商。站点不需要预先构建。仅需要源文件。在尝试部署之前，请确保先运行 read_deployment_config 工具，并已创建所有缺失的文件。如果你要部署到现有站点，请使用 project_id 标识该站点。如果你要部署新站点，请将 project_id 留空。",
  "name": "deploy_web_app",
  "parameters": {
    "properties": {
      "Framework": {
        "description": "Web 应用程序的框架。",
        "enum": ["eleventy", "angular", "astro", "create-react-app", "gatsby", "gridsome", "grunt", "hexo", "hugo", "hydrogen", "jekyll", "middleman", "mkdocs", "nextjs", "nuxtjs", "remix", "sveltekit", "svelte"],
        "type": "string"
      },
      "ProjectId": {
        "description": "Web 应用程序的项目 ID（如果它存在于部署配置文件中）。对于新站点或用户希望重命名站点的情况，请将此项留空。如果是重新部署，请在部署配置文件中查找项目 ID 并使用完全相同的 ID。",
        "type": "string"
      },
      "ProjectPath": {
        "description": "Web 应用程序的完整绝对项目路径。",
        "type": "string"
      },
      "Subdomain": {
        "description": "URL 中使用的子域名或项目名称。如果你使用 project_id 部署到现有站点，请将此项留空。对于新站点，子域名应具有唯一性且与项目相关。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 fd 在指定目录中搜索文件和子目录。\n搜索使用智能大小写匹配，默认情况下将忽略 gitignore 排除的文件。\nPattern 和 Excludes 均使用 glob 格式。如果你在按 Extensions 搜索，则无需同时指定 Pattern 和 Extensions。\n为避免输出过多，结果上限为 50 个匹配项。根据需要使用各种参数来过滤搜索范围。\n结果将包括类型、大小、修改时间和相对路径。",
  "name": "find_by_name",
  "parameters": {
    "properties": {
      "Excludes": {
        "description": "可选，排除与给定 glob 模式匹配的文件/目录",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "Extensions": {
        "description": "可选，要包含的文件扩展名（不带前导 .），匹配的路径必须至少与包含的扩展名之一匹配",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "FullPath": {
        "description": "可选，完整绝对路径是否必须与 glob 模式匹配，默认值：仅文件名需要匹配。启用此标志指定 glob 模式时要小心，例如当 FullPath 为开时，模式 '*.py' 将不匹配文件 '/foo/bar.py'，但模式 '**/*.py' 将匹配。",
        "type": "boolean"
      },
      "MaxDepth": {
        "description": "可选，最大搜索深度",
        "type": "integer"
      },
      "Pattern": {
        "description": "可选，要搜索的模式，支持 glob 格式",
        "type": "string"
      },
      "SearchDirectory": {
        "description": "要在其中进行搜索的目录",
        "type": "string"
      },
      "Type": {
        "description": "可选，类型过滤器，enum=file,directory,any",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用 ripgrep 在文件或目录中查找精确的模式匹配项。\n结果以 JSON 格式返回，对于每个匹配项，你将收到：\n- Filename\n- LineNumber\n- LineContent：匹配行的内容\n总结果上限为 50 个匹配项。使用 Includes 选项按文件类型或特定路径进行过滤，以缩小搜索范围。",
  "name": "grep_search",
  "parameters": {
    "properties": {
      "CaseInsensitive": {
        "description": "如果为 true，则执行不区分大小写的搜索。",
        "type": "boolean"
      },
      "Includes": {
        "description": "要在其中进行搜索的文件或目录。支持文件模式（例如，针对所有 .txt 文件的 '*.txt'）或特定路径（例如 'path/to/file.txt' 或 'path/to/dir'）。如果你在单个文件中进行 grep 操作，请将此项留空。",
        "items": {
          "type": "string"
        },
        "type": "array"
      },
      "MatchPerLine": {
        "description": "如果为 true，则返回与查询匹配的每一行，包括行号和匹配行的代码片段（等同于 'git grep -nI'）。如果为 false，则仅返回包含该查询的文件名（等同于 'git grep -l'）。",
        "type": "boolean"
      },
      "Query": {
        "description": "要在文件中查找的搜索词或模式。",
        "type": "string"
      },
      "SearchPath": {
        "description": "要搜索的路径。这可以是一个目录或一个文件。这是一个必需参数。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "列出目录的内容。目录路径必须是已存在的目录的绝对路径。对于目录中的每个子项，输出将包含：相对于该目录的相对路径、它是目录还是文件、如果是文件则显示以字节为单位的大小，如果是目录则显示子项数量（递归）。",
  "name": "list_dir",
  "parameters": {
    "properties": {
      "DirectoryPath": {
        "description": "要列出其内容的路径，应为指向目录的绝对路径",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "读取 Web 应用程序的部署配置，并确定该应用程序是否已准备好进行部署。应仅在为 deploy_web_app 工具做准备时使用。",
  "name": "read_deployment_config",
  "parameters": {
    "properties": {
      "ProjectPath": {
        "description": "Web 应用程序的完整绝对项目路径。",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "从 URL 读取内容。URL 必须是 HTTP 或 HTTPS URL，指向可通过 Web 浏览器访问的有效互联网资源。",
  "name": "read_url_content",
  "parameters": {
    "properties": {
      "Url": {
        "description": "要从中读取内容的 URL",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用此工具编辑现有文件。请务必遵守以下所有规则：\n1. 切勿针对同一个文件多次并行调用此工具。\n2. 要在同一个文件中编辑多个不相邻的代码行，请对此工具进行单次调用。将每次编辑指定为单独的 ReplacementChunk。\n3. 对于每个 ReplacementChunk，指定 TargetContent 和\tReplacementContent。在 TargetContent 中，指定要编辑的精确代码行。这些行必须与现有文件内容中的文本完全匹配。在 ReplacementContent 中，指定用于替换指定目标内容的内容。这必须是对 TargetContent 的完整直接替换，并进行了必要的修改。\n4. 如果你在单个文件中进行多处编辑，请指定多个单独的 ReplacementChunk。切勿尝试用新内容替换整个现有内容，这样开销非常大。\n5. 你不得编辑文件扩展名：[.ipynb]\n你应该在其他参数之前指定以下参数：[TargetFile]",
  "name": "replace_file_content",
  "parameters": {
    "properties": {
      "CodeMarkdownLanguage": {
        "description": "代码块的 Markdown 语言，例如 'python' 或 'javascript'",
        "type": "string"
      },
      "Instruction": {
        "description": "对你对文件所做更改的描述。",
        "type": "string"
      },
      "ReplacementChunks": {
        "description": "要替换的块列表。如果可能，最好为非连续编辑提供多个块。这必须是一个 JSON 数组，而不是字符串。",
        "items": {
          "additionalProperties": false,
          "properties": {
            "AllowMultiple": {
              "description": "如果为 true，当找到 'targetContent' 的多次出现时，将替换为 'replacementContent'。否则如果找到多次出现，将返回错误。",
              "type": "boolean"
            },
            "ReplacementContent": {
              "description": "用于替换目标内容的内容。",
              "type": "string"
            },
            "TargetContent": {
              "description": "要替换的精确字符串。这必须是要替换的精确字符序列，包括空格。请非常小心地包含任何前导空格，否则这将完全不起作用。如果 AllowMultiple 不为 true，则这必须是文件中的唯一子字符串，否则会报错。",
              "type": "string"
            }
          },
          "required": ["TargetContent", "ReplacementContent", "AllowMultiple"],
          "type": "object"
        },
        "type": "array"
      },
      "TargetFile": {
        "description": "要修改的目标文件。始终将目标文件指定为第一个参数。",
        "type": "string"
      },
      "TargetLintErrorIds": {
        "description": "如果适用，此编辑旨在修复的 lint 错误 ID（它们将在最近的 IDE 反馈中提供）。如果你认为此编辑可以修复 lints，请务必指定 lint ID；如果该编辑完全不相关，则不要指定。根据经验，如果你的编辑受 lint 反馈影响，请包含 lint ID。在此做出诚实的判断。",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "代表用户提议运行一条命令。操作系统：mac。Shell：bash。\n**切勿提议 cd 命令**。\n如果你拥有此工具，请注意你确实有能力直接在用户的系统上运行命令。\n请确保所指定的 CommandLine 与其应在 shell 中运行的形式完全一致。\n请注意，在执行命令之前，必须经过用户批准。如果命令不符合用户心意，用户可能会拒绝。\n在用户批准之前，实际命令不会执行。用户可能不会立即批准。\n如果步骤处于等待用户批准状态（WAITING），则表示它尚未开始运行。\n命令将在 PAGER=cat 环境变量下运行。对于通常依赖分页且可能包含极长输出的命令（例如 git log，可使用 git log -n <N>），你可能需要限制其输出长度。",
  "name": "run_command",
  "parameters": {
    "properties": {
      "Blocking": {
        "description": "若为 true，命令将阻塞直到完全运行完毕。在此期间，用户将无法与 Cascade 交互。仅在以下情况应设为 true：(1) 命令将在相对较短的时间内结束，或 (2) 在回复用户之前查看该命令的输出对你至关重要。否则，如果你正在运行一个长时间运行的进程（例如启动 Web 服务器），请将其设为非阻塞。",
        "type": "boolean"
      },
      "CommandLine": {
        "description": "要执行的确切命令行字符串。",
        "type": "string"
      },
      "Cwd": {
        "description": "命令的当前工作目录",
        "type": "string"
      },
      "SafeToAutoRun": {
        "description": "如果你认为此命令在未经用户批准的情况下安全可运行，请设为 true。如果命令可能产生某些破坏性副作用，则该命令是不安全的。不安全副作用的示例包括：删除文件、更改状态、安装系统依赖项、发出外部请求等。仅当你极其确信其安全时才设为 true。如果你觉得命令可能不安全，即使用户要求，也绝不要设为 true。切勿自动运行具有潜在不安全性的命令，这一点至关重要。",
        "type": "boolean"
      },
      "WaitMsBeforeAsync": {
        "description": "仅在 Blocking 为 false 时适用。它指定在启动命令后、将其转为完全异步之前等待的毫秒数。如果有某些命令应异步运行但可能会快速报错失败，这会很有用。这样能让你查看到在此时间段内发生的错误。不要设置得太长，否则可能会让所有人一直等待。",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "执行网络搜索，根据给定的查询和可选的域名过滤器获取相关网页文档的列表。",
  "name": "search_web",
  "parameters": {
    "properties": {
      "domain": {
        "description": "建议搜索优先考虑的可选域名",
        "type": "string"
      },
      "query": {
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "如果你未调用其他工具且正在向用户提问，请使用此工具为你的问题提供少量可能的建议答案。示例可以是 是/否，或其他简单的多选选项。请谨慎使用此工具，并且仅在你笃定期望从用户那里收到所建议选项之一时才使用。如果用户的下一次输入可能是包含更多细节的简短或长篇回复，则不要提供任何建议。例如，假设用户接受了你的建议回复：如果你随后还会提出另一个跟进问题，那么该建议就是糟糕的，你一开始就不应该提出。尽量不要连续多次使用此工具。",
  "name": "suggested_responses",
  "parameters": {
    "properties": {
      "Suggestions": {
        "description": "建议列表。每个建议最多几个词，返回选项不要超过 3 个。",
        "items": {
          "type": "string"
        },
        "type": "array"
      }
    },
    "type": "object"
  }
}

{
  "description": "查看代码项节点的内容，例如文件中的类或函数。你必须使用完全限定的代码项名称，例如由 grep_search 工具返回的那些名称。例如，如果你有一个名为 `Foo` 的类，并且你想查看 `Foo` 类中的函数定义 `bar`，则可以使用 `Foo.bar` 作为 NodeName。如果 codebase_search 工具之前已经显示过该内容，请不要请求查看该符号。如果文件中未找到该符号，该工具将返回一个空字符串。",
  "name": "view_code_item",
  "parameters": {
    "properties": {
      "File": {
        "description": "要编辑节点的绝对路径，例如 /path/to/file",
        "type": "string"
      },
      "NodePath": {
        "description": "节点在文件内的路径，例如 package.class.FunctionName",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "查看文件内容。文件行号基于 0 索引，此工具调用的输出将是从 StartLine 到 EndLine（包含）的文件内容，以及 StartLine 和 EndLine 之外行内容的摘要。请注意，此调用一次最多只能查看 200 行。\n\n使用此工具收集信息时，你有责任确保自己掌握完整上下文。具体来说，每次调用此命令时，你应当：\n1) 评估你查看的文件内容是否足以继续执行任务。\n2) 如果你查看的文件内容不足，且怀疑所需内容可能在未显示的行中，请主动再次调用该工具查看这些行。\n3) 如有疑问，请再次调用此工具以收集更多信息。请记住，部分文件视图可能会遗漏关键的依赖项、导入或功能。",
  "name": "view_file",
  "parameters": {
    "properties": {
      "AbsolutePath": {
        "description": "要查看的文件路径。必须是绝对路径。",
        "type": "string"
      },
      "EndLine": {
        "description": "要查看的结束行，包含该行。与 StartLine 的距离不能超过 200 行",
        "type": "integer"
      },
      "IncludeSummaryOfOtherLines": {
        "description": "若为 true，除了从 StartLine 到 EndLine 的确切代码行之外，你还将获得整个文件内容的精简摘要。",
        "type": "boolean"
      },
      "StartLine": {
        "description": "要查看的起始行",
        "type": "integer"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用网页文档的 URL 和分块位置查看特定的内容分块。在此 URL 上使用此工具之前，必须已通过 read_url_content 工具读取该 URL。",
  "name": "view_web_document_content_chunk",
  "parameters": {
    "properties": {
      "position": {
        "description": "要查看的分块位置",
        "type": "integer"
      },
      "url": {
        "description": "该分块所属的 URL",
        "type": "string"
      }
    },
    "type": "object"
  }
}

{
  "description": "使用此工具创建新文件。如果文件及其任何父目录尚不存在，系统将为你创建它们。\n\t\t请遵循以下说明：\n\t\t1. 切勿使用此工具修改或覆盖现有文件。在调用此工具之前，请务必先确认 TargetFile 不存在。\n\t\t2. 你必须将 TargetFile 指定为第一个参数。请在指定任何代码内容之前先指定完整的 TargetFile。\n你应该在其他参数之前指定以下参数：[TargetFile]",
  "name": "write_to_file",
  "parameters": {
    "properties": {
      "CodeContent": {
        "description": "要写入文件的代码内容。",
        "type": "string"
      },
      "EmptyFile": {
        "description": "设为 true 以创建一个空文件。",
        "type": "boolean"
      },
      "TargetFile": {
        "description": "要创建并写入代码的目标文件。",
        "type": "string"
      }
    },
    "type": "object"
  }
}
{/functions}
