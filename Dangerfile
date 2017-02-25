# Warn about develop branch
current_branch = env.request_source.pr_json["base"]["ref"]
warn("Please target PRs to `develop` branch") if current_branch != "develop"
# Sometimes it's a README fix, or something like that - which isn't relevant for
# including in a project's CHANGELOG for example
declared_trivial = github.pr_title.include? "#trivial"

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn("PR is classed as Work in Progress") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > 500

# Don't let testing shortcuts get into master by accident
fail("fdescribe left in tests") if `grep -r fdescribe specs/ `.length > 1
fail("fit left in tests") if `grep -r fit specs/ `.length > 1

# If these are all empty something has gone wrong, better to raise it in a comment
if git.modified_files.empty? && git.added_files.empty? && git.deleted_files.empty?
  fail "This PR has no changes at all, this is likely a developer issue."
end

# Warn summary on pull request
if github.pr_body.length < 5
  warn "Please provide a summary in the Pull Request description"
end

if git.commits.any? { |c| c.message =~ /^Merge branch/ }
  fail 'Please rebase to get rid of the merge commits in this PR'
end
