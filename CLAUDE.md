## Astra Upcheck

This project uses the [Astra](https://github.com/jaimeam/astra) programming language.

### Setup (run once)

```bash
# Install Astra (requires Rust 1.70+)
git clone https://github.com/jaimeam/astra.git /tmp/astra
cd /tmp/astra && cargo build --release
export PATH="$PATH:/tmp/astra/target/release"
```

### Key Commands

```bash
astra check src/              # Type-check all files (must pass with 0 errors)
astra check --json src/       # Same, with machine-readable JSON output
astra test                    # Run all tests (must pass)
astra fix src/                # Auto-apply suggested fixes
astra fmt src/                # Format all code canonically
astra run src/main.astra      # Run the program
astra explain E1001           # Explain any error code
```

### Development Workflow

1. Write or edit `.astra` files
2. Run `astra check src/` — fix any errors
3. Run `astra test` — fix any failures
4. Run `astra fmt src/` — format before committing

### Language Quick Reference

- Every file starts with `module <name>`
- Functions: `fn name(param: Type) -> ReturnType { body }`
- Side effects must be declared: `fn name() effects(Console, Fs) { ... }`
- No null — use `Option[T]` with `Some(value)` / `None`
- Errors use `Result[T, E]` with `Ok(value)` / `Err(error)`
- Export with `public fn`, import with `import module.{name}`
- Comments use `#`
- No semicolons
