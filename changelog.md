# Changelog

All notable changes to this project will be documented in this file.

## [2.2.0] - 2026-03-29

### Added
- User configuration file (`user-config.yaml`) for easy setup
- Dynamic path configuration (no hardcoded paths)
- Notification configuration support
- `.gitignore` for cleaner releases

### Changed
- **Breaking**: Removed hardcoded paths from all files
- **Breaking**: Changed `owner` references to `{owner}` placeholder
- Setup script now reads from user config
- All prompts use dynamic paths from config.yaml

### Removed
- Internal team references ({main_agent}, Nex, etc.)
- Hardcoded Telegram ID
- Hardcoded Windows paths

## [2.1.0] - 2026-03-27

### Added
- Multi-output channels (blog, methodology, knowledge base)
- High-value refinement workflow
- Memory tier promotion/demotion

### Changed
- Merged evaluator into cron-trigger
- Merged classifier into distill-classifier

## [2.0.0] - 2026-03-19

### Added
- Modular architecture with pluggable modules
- Checkpoint-based recovery system
- Three-layer memory (HOT/WARM/COLD)
- Approval workflow for system changes

### Changed
- Complete rewrite from v1.x
- YAML-based configuration
- Separated prompts from modules

## [1.0.0] - 2026-03-01

### Added
- Initial release
- Basic feedback collection
- Simple rule extraction
- Corrections log
