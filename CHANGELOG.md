# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.3] - 2026-02-19

### Added
- `machine_id` field to Task schema (defaults to "default")
- `working_directory` field to Task schema (required, base CWD for task execution)

### Fixed
- Updated all tests to work with new required fields
- Fixed metadata undefined issue in createTask() method
- All 69 unit tests now passing (100%)

## [1.0.2] - 2026-02-19

### Added
- `removeMilestone()` method in TaskDatabaseService to delete milestones from tasks
- `removeTaskItem()` method in TaskDatabaseService to delete task items from milestones
- `removeMilestone()` method in FirebaseClient for user-scoped milestone deletion
- `removeTaskItem()` method in FirebaseClient for user-scoped task item deletion

## [1.0.0] - 2026-02-18

### Added
- Initial release of task-core library
- Zod schemas for Task, Milestone, TaskItem
- DTO types and transformer functions
- TaskDatabaseService for Firestore operations
- FirebaseClient wrapper for multi-tenant access
- Collection path helpers

[Unreleased]: https://github.com/yourusername/task-core/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/yourusername/task-core/releases/tag/v1.0.0
