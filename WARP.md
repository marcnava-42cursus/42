# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Overview

This is a 42 School curriculum repository containing multiple C programming projects organized by milestones. Each project is implemented as a git submodule with its own independent repository structure.

## Architecture

### Repository Structure
- **src/**: Contains all project directories organized by curriculum milestones
- **Milestone 0**: `libft` - Custom C library implementation
- **Milestone 1**: `ft_printf`, `get_next_line` - File I/O and formatted output
- **Milestone 2**: `push_swap`, `pipex`, `minitalk`, `so_long`, `fract-ol` - Algorithms, IPC, graphics
- **Milestone 3**: `philosophers`, `Minishell` - Threading, shell implementation
- **Milestone 4**: `cpp` - C++ modules (cpp00-09)
- **Milestones 5-6**: Reserved for advanced projects

### Project Dependencies
- Most projects depend on `libft` as a foundational library
- Projects use standard 42 School Makefile patterns with `all`, `clean`, `fclean`, `re` targets
- Build artifacts are typically stored in `build/` or similar directories
- Headers are conventionally placed in `includes/` directories

## Common Commands

### Building Projects
```bash
# Build any individual project
cd src/[project_name]
make

# Clean build artifacts
make clean    # Remove object files
make fclean   # Remove all build artifacts including binaries
make re       # Clean rebuild (fclean + all)
```

### Working with Submodules
```bash
# Initialize all submodules
git submodule update --init --recursive

# Update a specific submodule
git submodule update --remote src/[project_name]

# Clone with submodules
git clone --recurse-submodules [repo_url]
```

### Dependency Management
```bash
# Minishell: libft is auto-cloned during build
cd src/Minishell
make  # Automatically clones libft to libs/libft if not present

# Clean dependencies
make fclean  # Removes binary and cleans libft
rm -rf libs  # Remove cloned libraries completely

# Manual libft management
git clone https://github.com/marcnava-42cursus/libft.git libs/libft
make -C libs/libft
```

### Testing
```bash
# Minishell comprehensive testing
cd src/Minishell/tests
./comprehensive_test.sh     # Most complete test with exit codes, memory leaks
./run_specific_tests.sh basic      # Run specific test categories
./run_specific_tests.sh builtin    # Test built-in commands
./run_specific_tests.sh pipeline   # Test pipes and pipelines
./run_specific_tests.sh redirect   # Test redirections
./run_specific_tests.sh memory     # Memory leak detection
./compare_with_bash.sh      # Compare behavior with bash
./valgrind_test_minishell.sh # Memory leak testing
./stress_test_minishell.sh  # Stress testing

# Legacy/additional testing
./run_all_tests.sh          # Run all available tests
./focused_test_minishell.sh # Focused testing
./test_pipes.sh            # Pipe-specific tests

# Push_swap testing
cd src/push_swap/mandatory
./push_swap_test_linux.sh

# Philosophers testing
cd src/philosophers
./test.sh
```

### Development Workflow
```bash
# Minishell development workflow
cd src/Minishell
make check                  # Quick compilation validation (✅ Todo OK)
make                       # Build with address sanitizer enabled
./minishell               # Run interactive shell

# Testing workflow
make && ./tests/comprehensive_test.sh  # Build and run comprehensive tests
./tests/run_specific_tests.sh --help   # View available test categories

# Memory debugging (common pattern)
valgrind --leak-check=full ./minishell
valgrind --leak-check=full --show-leak-kinds=all ./minishell

# Debug specific components
make clean && make CFLAGS="-Wall -Wextra -g3"  # Debug build without sanitizer
gdb ./minishell
```

## Key Technical Details

### Compilation Standards
- All C projects use: `gcc -Wall -Wextra -Werror`
- Some projects include additional flags like `-fsanitize=address` for debugging
- Library creation uses: `ar rcs`

### Project-Specific Notes

#### libft
- Core library providing reimplemented standard C functions
- Organized into modules: aux, conversions, fdout, ft_printf, get_next_line, libc, ctype, lst, math
- Essential dependency for most other projects

#### Unique Minishell Features
- **Advanced parsing**: AST-based parser with complete bash syntax compatibility
- **Command suggestions**: Dynamic command completion system with PATH scanning  
- **Wildcard expansion**: Full glob pattern matching for `*` wildcards
- **Configuration system**: Customizable prompts, colors, and shell behavior
- **Comprehensive testing**: 10+ specialized test scripts covering all edge cases
- **Memory safety**: Built-in address sanitizer integration for development
- **Easter egg**: Hidden Forkerman game accessible via special commands

#### Minishell  
- Requires readline and ncurses libraries (`-lreadline -lncurses`)
- **Architecture**: Modular design with separate components:
  - `config/`: Build prompt, colors, config management, keywords
  - `exec/`: Command execution, pipelines, builtins integration
  - `parser/`: Input parsing, expansions, AST tree construction
  - `structs/`: Environment and tree management
  - `suggestions/`: Command autocompletion system
  - `utils/`: I/O, matrix operations, terminal control
  - `wildcards/`: Glob pattern matching (`*` expansion)
  - `forkerman/`: Easter egg game implementation
- **Features**: Full bash compatibility including heredocs, pipes, redirections
- Uses dynamic library cloning for libft dependency
- Advanced features: wildcards, command suggestions, configuration system
- **Build flags**: Includes address sanitizer (`-fsanitize=address`) for debugging

#### Graphics Projects (so_long, fract-ol, cub3d)
- Require MinilibX graphics library
- Texture and map file dependencies
- Window management and event handling

#### Threading Projects (philosophers)
- Careful mutex and thread management
- Timing-critical implementations
- Extensive testing for race conditions and deadlocks

### Common Patterns
- Most projects follow the structure: `src/`, `includes/`, `build/`, `Makefile`
- Bonus implementations often in `_bonus/` directories
- Testing scripts typically in `tests/` or project root
- Header files use standard 42 header format with creation/update timestamps

## Development Tips

### Debugging
- Use `-g3` flag for detailed debugging information
- Address sanitizer is commonly enabled in development builds
- Valgrind testing is standard practice for memory management verification

### Code Organization
- Functions are typically organized by logical grouping in separate files
- Header files contain function prototypes and structure definitions
- Makefile dependency management ensures proper compilation order

### Testing Strategy
- Projects often include multiple test suites (unit, integration, comparison)
- Custom shell scripts automate testing workflows
- Third-party testers are commonly used (e.g., LazyPhilosophersTester, minishell_tester)