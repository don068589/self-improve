# Contributing to Self-Improve

Thank you for your interest in contributing!

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](https://github.com/openclaw/self-improve/issues) to report bugs
- Include steps to reproduce the problem
- Mention your OpenClaw and Node.js versions

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### Code Style

- Use ES modules (`.mjs` extension)
- Follow existing code patterns
- Add comments for complex logic

### Module Development

When adding or modifying modules:

1. Read `modules/TEMPLATE.md` for the required format
2. Create a `MODULE.md` in your module directory
3. Add a corresponding prompt in `prompts/`
4. Register in `config.yaml`
5. Update `changelog.md`

### Prompt Development

Prompts are the core of this system. When writing prompts:

- Be explicit and specific
- Include examples
- Define input/output clearly
- Test with various scenarios

## Questions?

Open an issue or join the [OpenClaw Community](https://discord.com/invite/clawd).
