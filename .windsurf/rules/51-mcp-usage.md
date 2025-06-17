# 51: MCP (Model Context Protocol) Usage Rules

**Preamble:** Rules governing the use of MCP tools.

## Rules
51.1. **Intentionality:** Only use a tool when you have a clear and specific purpose for it. Do not use tools for exploratory "fishing expeditions."
51.2. **Tool Selection:** Choose the most appropriate tool for the job. For example, use `read_file` instead of `execute_command('cat file.txt')`.
51.3. **Parameter Precision:** Provide accurate and specific parameters to tools. For file paths, use absolute paths whenever possible.
51.4. **Output Analysis:** After a tool runs, carefully analyze its output. If the output is unexpected or indicates an error, address it before proceeding.
51.5. **Resource Awareness:** Be mindful of resource-intensive tools (e.g., those that make external API calls). Use them efficiently and avoid redundant calls.