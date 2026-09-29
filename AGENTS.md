# Repository guidance

Before working, read these optional instruction files in order,
resolving these paths from this repository's root:

1. `.agents/organization/AGENTS.md`
2. `.agents/workspace/AGENTS.md`

Read each file if it resolves to a readable regular file.
Skip absent files; report broken or unreadable links.
Read each resolved file only once to avoid duplicate loading and cycles.

Resolve references inside imported files relative to their real target
directory after following symlinks.

Apply organization guidance, then workspace guidance, then the repository
instructions below. More specific applicable instructions take precedence.

## Repository scope

- This is Krestomatio's Moodle-core fork. It follows the supported Moodle LTS
  line; use the operational branch declared by workspace inventory rather than
  assuming a generic remote default is the production base.
- During an LTS upgrade, current and next service branches may both be active.
  Keep fixes and release pins aligned with the intended line.
- The Krestomatio delta supplies `core_user\hook\before_user_created` in
  `user/classes/hook/before_user_created.php` and dispatches it from
  `user/lib.php`. `moodle-local_tier` depends on that pre-insert hook to reject
  registrations beyond a plan limit.
- Keep the service patch minimal, tested, and easy to rebase onto the applicable
  Moodle LTS maintenance releases. Avoid unrelated upstream formatting or
  generated churn.

## Validation and delivery

- Follow upstream Moodle coding/test guidance in `CONTRIBUTING.md`. At minimum,
  PHP-lint changed files and run the narrow PHPUnit/Behat/Grunt checks for affected
  components; use the full upstream suite when the change warrants it.
- Test the hook contract with `moodle-local_tier`, including failure before user
  insertion. Updating a service branch does not deploy it: update the matching
  pin in `container_builder`, rebuild the corresponding LTS image, then update
  the deployment digest.
