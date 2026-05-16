# Serena MCP Server Configuration

## ✅ Configuration Complete

The Serena MCP server has been successfully configured for the Tejo Beauty Enterprise Platform project.

## Project Information

- **Project Name:** corgi13 (Tejo Beauty)
- **Project Path:** `c:\Users\patri\source\repos\corgi13`
- **Primary Language:** TypeScript
- **Architecture:** Nx monorepo + pnpm workspace
- **Serena Version:** 0.1.4

## Active Configuration

### Context

- **Active Context:** claude-code
- **Active Modes:** interactive, editing

### Available Tools

The following Serena tools are active and ready to use:

**Project Management:**

- `activate_project` - Switch between projects
- `get_current_config` - View current configuration

**Memory Management:**

- `write_memory` - Save project knowledge
- `read_memory` - Retrieve saved knowledge
- `edit_memory` - Update existing memories
- `delete_memory` - Remove outdated memories
- `list_memories` - View all saved memories

**Code Navigation:**

- `get_symbols_overview` - Get high-level view of file symbols
- `find_symbol` - Search for specific code symbols
- `find_referencing_symbols` - Find where symbols are used
- `list_dir` - List directory contents
- `find_file` - Search for files

**Code Editing (Symbolic):**

- `replace_symbol_body` - Replace entire symbol definition
- `insert_after_symbol` - Add code after a symbol
- `insert_before_symbol` - Add code before a symbol
- `rename_symbol` - Rename symbols across codebase

**Search:**

- `search_for_pattern` - Flexible pattern search in codebase

**Workflow:**

- `think_about_collected_information` - Reflect on gathered context
- `think_about_task_adherence` - Check alignment with task goals
- `think_about_whether_you_are_done` - Assess task completion

## Project Memories

The following memory files have been created to provide context about the project:

### 1. `project_overview.md`

- Project purpose and goals
- Complete tech stack
- Architecture overview
- Design philosophy

### 2. `suggested_commands.md`

- Essential development commands
- Database management scripts
- Testing and verification commands
- Windows system utilities

### 3. `code_style_conventions.md`

- TypeScript standards
- React/Next.js conventions
- NestJS backend patterns
- Formatting rules (Prettier)
- ESLint configuration
- Git commit conventions
- Documentation standards

### 4. `task_completion_workflow.md`

- Complete checklist for finishing tasks
- Code quality verification steps
- Testing procedures
- Pre-commit checks
- Git workflow

### 5. `project_structure.md`

- Detailed directory organization
- Monorepo structure (Nx + pnpm)
- Migration status (legacy → canonical)
- Important file locations
- Port assignments

### 6. `design_patterns.md`

- Architectural patterns (layered, DI, etc.)
- Component patterns (atomic design)
- State management strategies
- Error handling approaches
- Performance patterns
- Security patterns

## How to Use Serena

### Basic Workflow

1. **Understand Code Structure:**

   ```
   Use: get_symbols_overview → find_symbol → find_referencing_symbols
   ```

2. **Search for Code:**

   ```
   Use: search_for_pattern or find_symbol (with pattern matching)
   ```

3. **Edit Code:**

   ```
   Use: replace_symbol_body or insert_after_symbol/insert_before_symbol
   ```

4. **Navigate Project:**
   ```
   Use: list_dir → find_file
   ```

### Best Practices

✅ **DO:**

- Use symbolic tools for code exploration (avoid reading entire files)
- Read memories when they're relevant to your current task
- Use pattern search when you don't know exact symbol names
- Think about collected information before making changes
- Use symbolic editing for complete function/class replacements

❌ **DON'T:**

- Read entire files unless absolutely necessary
- Skip verification after making changes
- Ignore the memories - they contain project-specific knowledge
- Use line-based editing for symbol-level changes

### Memory Usage Guidelines

**When to read memories:**

- Starting a new task in unfamiliar area
- Need to know coding conventions
- Need to understand project structure
- Need to know what commands to run

**Memory file quick reference:**

- Need commands? → `suggested_commands.md`
- Need style guide? → `code_style_conventions.md`
- Need architecture info? → `project_structure.md` or `design_patterns.md`
- Need completion checklist? → `task_completion_workflow.md`
- Need project overview? → `project_overview.md`

## Integration with VS Code

Serena is now available in your coding environment. The agent (GitHub Copilot) can use these tools to:

- Intelligently navigate and understand the codebase
- Make precise symbolic edits to code
- Access project-specific knowledge from memories
- Follow established conventions and patterns
- Provide context-aware assistance

## Updating Configuration

To update Serena configuration or memories:

1. **Add new memory:**

   ```typescript
   mcp_oraios_serena_write_memory({
     content: "...",
     memory_file_name: "new_memory.md",
   });
   ```

2. **Edit existing memory:**

   ```typescript
   mcp_oraios_serena_edit_memory({
     memory_file_name: "existing.md",
     // edit operations
   });
   ```

3. **Switch modes:** Contact Serena administrator or update config

## Troubleshooting

### Common Issues

**Problem:** Tools not available

- **Solution:** Check `get_current_config` - tools may be disabled by mode

**Problem:** Can't find symbols

- **Solution:** Ensure you're searching in the correct file path (relative to project root)

**Problem:** Memory not found

- **Solution:** Use correct memory file name (check list above)

**Problem:** Edits not working

- **Solution:** Verify symbol name_path format matches language conventions

## Additional Resources

- **Serena Documentation:** https://github.com/oraios/serena
- **Project Docs:** `tejospec/docs/MASTER_PLAN.md`
- **Architecture:** `tejospec/docs/IMPROVEMENT_ROADMAP.md`
- **Status:** `tejospec/SYSTEM_STATUS_DEC26.md`

---

**Configuration Date:** January 4, 2026  
**Configured By:** AI Assistant  
**Status:** ✅ Active and Ready
