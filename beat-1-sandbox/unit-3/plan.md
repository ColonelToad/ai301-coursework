# Implementation plan: Issue #18 — provide file paths to repository analysis

## Goal

Issue #18 reports that `RepoAnalyzer` always reports `has_tests` and `has_ci` as `False` because it receives no repository file list under `file_structure`.

The reproduced source path supports that behavior.

This change will make `GitHubTool` retrieve repository file paths and return them as `file_structure`, allowing the existing analyzer detection logic to evaluate the paths.


`_detect_ci()` checks `file_structure` for `.github/workflows`, `.travis.yml`, `.circleci`, or `gitlab-ci`. `_detect_tests()` checks it for test-path and test-configuration indicators including `tests/`, `test/`, `pytest.ini`, `test_`, `__tests__`, and `spec/`.

## Scope

### In scope

- Extend `GitHubTool._fetch_repo_metadata()` in `agent/tools/github_tool.py` to retrieve repository file paths with a GitHub recursive tree request.
- Extract returned file paths and add them to the existing metadata dictionary under `file_structure`.
- Keep the existing `RepoAnalyzer` detection heuristics and supply the data format those methods already consume.
- Add focused coverage demonstrating that representative test and CI paths lead to `has_tests=True` and `has_ci=True`.
- Capture before/after evidence by rerunning the Unit 2 reproduction path or the closest real code path built from it.

### Out of scope

- Redesigning or expanding `_detect_tests()` and `_detect_ci()` indicator heuristics.
- Refactoring the broader metadata-key differences between `GitHubTool` output and the other repository fields that `RepoAnalyzer` reads.
- Changing unrelated ingestion, repository scoring, authentication, or GitHub API behavior.
- Building a general-purpose repository crawler beyond the single recursive file-tree request required for this issue.

## Planned changes

1. **`agent/tools/github_tool.py` — `GitHubTool._fetch_repo_metadata()`**
   - Keep the existing `GET /repos/{owner}/{repo}` metadata request.
   - Read the repository’s `default_branch` from the metadata response, falling back safely if it is absent.
   - Request the repository’s Git tree recursively for that branch.
   - Extract non-empty `path` values from the returned tree entries.
   - Add the resulting path collection to the returned metadata as `"file_structure"`.
   - If the file-tree request fails or provides no usable paths, preserve the tool’s current safe behavior by returning an empty file structure instead of failing otherwise successful metadata retrieval.
   - Record or handle a truncated tree response according to the project’s existing logging/error-handling conventions, rather than treating it as a guaranteed complete repository listing.


2. **`ingestion/parsers/repo_analyzer.py` — no planned logic change**
   - Leave `_detect_tests()` and `_detect_ci()` unchanged because they already evaluate `file_structure`.
   - Make a narrow compatibility adjustment only if the focused tests demonstrate that the verified tree-path representation does not work with the existing string conversion.
   - Record that as a deviation if needed.

## Validation

1. Run the Unit 2 reproduction path before implementation with input containing recognizable test and CI paths. Save the command and output showing the absent/empty `file_structure` input and false flags.
2. Implement the GitHub file-tree retrieval and metadata handoff.
3. Rerun the exact same reproduction path, or the closest focused test through the real `GitHubTool` to `RepoAnalyzer` behavior, using the same representative paths.
4. Confirm that the post-change output includes `file_structure` with the expected paths and that `has_tests=True` and `has_ci=True`.


## Assumptions and open questions

- I will verify the repository’s existing HTTP mocking and test conventions before selecting the exact test files and commands.
- I will verify how the application passes `GitHubTool` output into `RepoAnalyzer`; if no direct production composition exists, focused tool and parser tests will be the closest executable path.
- The GitHub tree response may be truncated for large repositories. I will preserve safe behavior and document any limitation that the project’s existing conventions require.
- The visible metadata-key mismatch beyond `file_structure` is not part of this bounded Issue #18 change.

## Deviations

No implementation work has started. I will update this section after building. If the implementation follows this plan, I will record: “Nothing changed; the plan held.” If the source locations, API request approach, test approach, validation path, or scope changes, I will state what changed and why.
