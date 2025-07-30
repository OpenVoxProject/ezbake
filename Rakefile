begin
  require 'github_changelog_generator/task'

  GitHubChangelogGenerator::RakeTask.new :changelog do |config|
    config.exclude_labels = %w{duplicate question invalid wontfix wont-fix skip-changelog}
    config.user = 'OpenVoxProject'
    config.project = 'ezbake'
    # Patches welcome to improve this
    # Also lives in openvoxdb/openvox-server
    config.future_release = File.readlines('project.clj').first.scan(/".*"/).first.gsub('"', '')
    config.release_branch = 'main'
    config.since_tag = '2.6.2'
  end
rescue LoadError
  # Optional group in bundler
end
