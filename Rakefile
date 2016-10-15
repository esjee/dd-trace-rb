require 'bundler/gem_tasks'
require 'rubocop/rake_task'
require 'rake/testtask'
require 'rdoc/task'
require 'appraisal'

Rake::TestTask.new(:test) do |t|
  t.libs << %w(test lib)
  t.test_files = FileList['test/**/*_test.rb'].reject do |path|
    path.include?('rails')
  end
end

Rake::TestTask.new(:rails) do |t|
  t.libs << %w(test lib)
  t.test_files = FileList['test/contrib/rails/**/*_test.rb']
end

RuboCop::RakeTask.new(:rubocop) do |t|
  t.patterns = ['lib/**/*.rb', 'test/**/*.rb', 'Gemfile', 'Rakefile']
end

RDoc::Task.new(:rdoc) do |doc|
  doc.main   = 'docs/Getting_started.rdoc'
  doc.title  = 'Datadog Ruby Tracer'
  # TODO[manu]: include all lib/ folder, but only when all classes' docs are ready
  doc.rdoc_files = FileList.new(%w(lib/ddtrace/tracer.rb lib/ddtrace/span.rb docs/**/*.rdoc))
  doc.rdoc_dir = 'html'
end

# Deploy tasks
S3_BUCKET = 'gems.datadoghq.com'.freeze
S3_DIR = ENV['S3_DIR']

desc 'release the docs website'
task :'release:docs' => :rdoc do
  raise 'Missing environment variable S3_DIR' if !S3_DIR || S3_DIR.empty?
  sh "aws s3 cp --recursive html/ s3://#{S3_BUCKET}/#{S3_DIR}/docs/"
end

task default: :test