# frozen_string_literal: true

begin
  require 'github_changelog_generator/task'
rescue LoadError
  task :changelog do
    abort('Run `bundle install --with release` to install the `github_changelog_generator` gem.')
  end
else
  GitHubChangelogGenerator::RakeTask.new :changelog do |config|
    config.header = <<~HEADER.chomp
      # Changelog

      All notable changes to this project will be documented in this file.
    HEADER
    config.user = 'OpenVoxProject'
    config.project = 'ezbake'
    config.exclude_labels = %w[dependencies duplicate question invalid wontfix wont-fix modulesync skip-changelog]
    config.future_release = File.readlines('project.clj')
      .grep(/^\(defproject /).first[/"([^"]+)"/, 1]
      .sub(/-SNAPSHOT\z/, '')
    # The generated changelog starts with 2.7.0, the first release after the move to OpenVoxProject.
    # Only tags on main count. The 2.x branch continues from 2.7.9, and 3.0.0 was tagged off main.
    config.since_tag = '2.6.2'
    config.include_tags_regex = /\A2\.6\.2\z|\A2\.7\.[0-8]\z|\A4\./
    config.release_branch = 'main'
  end
end

namespace :vox do
  desc 'Set the version in project.clj for the release'
  task 'version:bump:full', [:version] do |_, args|
    version = args[:version]
    abort 'You must provide a version.' if version.nil? || version.empty?

    project = File.read('project.clj')
    defproject = /^(\(defproject\s+\S+\s+)"[^"]+"/
    abort 'Could not find the defproject version in project.clj' unless project.match?(defproject)

    File.write('project.clj', project.sub(defproject) { "#{Regexp.last_match(1)}\"#{version}\"" })
    puts "Set the version in project.clj to #{version}"
  end
end

desc 'Prepare for a release'
task 'release:prepare' => [:changelog]
