# frozen_string_literal: true
# encoding: UTF-8

require "fileutils"
require "yaml"
require "time"
require "digest"

SOURCE = "source".freeze

CONFIG = {
  "posts" => File.join(SOURCE, "_posts"),
  "post_ext" => "md"
}.freeze

ASSET_HASH_OUTPUT = "_config.assets.yml".freeze
FONT_PARTIAL_OUTPUT = File.join(SOURCE, "_sass", "end2end", "_fonts.sass")
ASSET_HASH_SOURCES = {
  "asset_hash_end2end_woff2" => File.join(SOURCE, "fonts", "end2end.woff2"),
  "asset_hash_main_css" => [File.join(SOURCE, "css", "main.sass"), FONT_PARTIAL_OUTPUT]
}.freeze
SECRETS_OUTPUT = "_config.secrets.yml".freeze

desc "List available tasks"
task :help do
  puts "Available tasks:"
  sh "rake -T"
end

task default: :help

desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless File.directory?(CONFIG["posts"])

  title = ENV["title"] || "New post"
  tags = build_list(ENV["tags"])
  categories = build_list(ENV["category"])
  slug = mount_slug(title)

  begin
    parsed_time = ENV["date"] ? Time.parse(ENV["date"]) : Time.now
  rescue StandardError
    abort("Error: date format must be YYYY-MM-DD or a valid parseable datetime.")
  end

  date = parsed_time.strftime("%Y-%m-%d")
  time = parsed_time.strftime("%T")
  filename = File.join(CONFIG["posts"], "#{date}-#{slug}.#{CONFIG['post_ext']}")

  if File.exist?(filename) && !confirm_overwrite?("#{filename} already exists. Do you want to overwrite? [y/N]")
    abort("rake aborted!")
  end

  puts "Creating new post: #{filename}"

  File.open(filename, "w") do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts %(title: "#{title.tr('-', ' ')}")
    post.puts "permalink: #{slug}"
    post.puts "date: #{date} #{time}"
    post.puts "comments: true"
    post.puts %(description: "#{title}")
    post.puts 'keywords: ""'
    post.puts "categories:"
    post.puts categories
    post.puts "tags:"
    post.puts tags
    post.puts "---"
  end
end

desc "Create a new page"
task :page do
  name = ENV["name"] || "new-page.md"
  filename = File.join(SOURCE, name)
  filename = File.join(filename, "index.html") if File.extname(filename).empty?

  title = File.basename(filename, File.extname(filename))
              .gsub(/[\W_]/, " ")
              .gsub(/\b\w/) { |char| char.upcase }

  if File.exist?(filename) && !confirm_overwrite?("#{filename} already exists. Do you want to overwrite? [y/N]")
    abort("rake aborted!")
  end

  slug = mount_slug(title)

  FileUtils.mkdir_p(File.dirname(filename))
  puts "Creating new page: #{filename}"

  File.open(filename, "w") do |page|
    page.puts "---"
    page.puts "layout: page"
    page.puts %(title: "#{title}")
    page.puts 'description: ""'
    page.puts 'keywords: ""'
    page.puts %(permalink: "#{slug}")
    page.puts %(slug: "#{slug}")
    page.puts "---"
  end
end

desc "Build the site and validate generated HTML/internal links"
task :check_links do
  sh "bundle exec jekyll build --config _config.yml"

  swap_urls = htmlproofer_swap_urls
  command = "bundle exec htmlproofer ./_site --disable-external --no-enforce-https"
  command += " --swap-urls '#{swap_urls}'" unless swap_urls.empty?

  sh command
end

desc "Generate asset hashes and the WOFF2 font partial for cache busting"
task "assets:hashes" do
  write_font_partial(FONT_PARTIAL_OUTPUT)
  write_asset_hash_config(ASSET_HASH_OUTPUT, ASSET_HASH_SOURCES)
end

desc "Generate secrets config from environment variables with per-variable validation"
task "config:secrets" do
  write_secrets_config(SECRETS_OUTPUT)
end

desc "Install dependencies, generate assets, and start a local Jekyll server"
task :serve => "assets:hashes" do
  sh "bundle install"
  sh "bundle exec jekyll serve --config _config.yml,_config.dev.yml"
end

desc "Validate a value using a reusable check"
task "validate:value", [:name, :value, :kind, :pattern] do |_task, args|
  validate_variable_value(
    args[:name],
    args[:value],
    kind: (args[:kind] || "regex").to_sym,
    pattern: args[:pattern],
    on_invalid: :abort,
    required: true
  )

  puts "Valid #{args[:name]}"
end

def confirm_overwrite?(message)
  print "#{message} "
  answer = $stdin.gets&.strip&.downcase
  answer == "y" || answer == "yes"
end

def mount_slug(title)
  str_clean(title)
    .downcase
    .strip
    .gsub(" ", "-")
    .gsub(/[^\w-]/, "")
end

def str_clean(title)
  title.tr(
    "ÀÁÂÃÄÅàáâãäåĀāĂăĄąÇçĆćĈĉĊċČčÐðĎďĐđÈÉÊËèéêëĒēĔĕĖėĘęĚěĜĝĞğĠġĢģĤĥĦħÌÍÎÏìíîïĨĩĪīĬĭĮįİıĴĵĶķĸĹĺĻļĽľĿŀŁłÑñŃńŅņŇňŉŊŋÒÓÔÕÖØòóôõöøŌōŎŏŐőŔŕŖŗŘřŚśŜŝŞşŠšſŢţŤťŦŧÙÚÛÜùúûüŨũŪūŬŭŮůŰűŲųŴŵÝýÿŶŷŸŹźŻżŽž",
    "AAAAAAaaaaaaAaAaAaCcCcCcCcCcDdDdDdEEEEeeeeEeEeEeEeEeGgGgGgGgHhHhIIIIiiiiIiIiIiIiIiJjKkkLlLlLlLlLlNnNnNnNnnNnOOOOOOooooooOoOoOoRrRrRrSsSsSsSssTtTtTtUUUUuuuuUuUuUuUuUuUuWwYyyYyYZzZzZz"
  )
end

def build_list(value)
  return "" if value.nil? || value.strip.empty?

  value.split(",").map { |item| "  - #{item.strip}" }.join("\n")
end

def htmlproofer_swap_urls
  baseurl = ENV["BASEURL"] || site_baseurl
  return "" if baseurl.nil? || baseurl.strip.empty?

  "^#{Regexp.escape(baseurl.strip)}/:/"
end

def site_baseurl
  config = YAML.load_file(File.join(Dir.pwd, "_config.yml"))
  config["baseurl"].to_s
end

def write_asset_hash_config(output_path, sources)
  source_paths = sources.values.flatten
  missing_sources = source_paths.reject { |source| File.file?(source) }

  unless missing_sources.empty?
    abort("rake aborted: missing asset file(s): #{missing_sources.join(', ')}")
  end

  content = sources.map do |key, source|
    %(#{key}: "#{digest_asset_source(source)}")
  end.join("\n")

  File.write(output_path, "#{content}\n")

  validate_yaml!(output_path, fatal: true)

  puts "Wrote #{output_path}"
end

def write_font_partial(output_path)
  # Keep the generated Sass partial free of Liquid so Sass handles compilation directly.
  woff2_path = File.join(SOURCE, "fonts", "end2end.woff2")
  woff2_hash = Digest::SHA256.file(woff2_path).hexdigest

  content = <<~SASS
    /* Using WOFF2 only (98%+ browser support). Remove TTF fallback for simplicity. */
    @font-face
      font-family: "End2End"
      src: url("../fonts/end2end.woff2?v=#{woff2_hash}") format("woff2")
      font-weight: 400
      font-style: normal
      font-display: swap
  SASS

  FileUtils.mkdir_p(File.dirname(output_path))
  File.write(output_path, content)
end

def write_secrets_config(output_path)
  secrets = []

  ga_measurement_id = validated_ga_measurement_id
  secrets << %(ga_measurement_id: "#{ga_measurement_id}") unless ga_measurement_id.nil?

  ga_options = validated_ga_options
  unless ga_options.nil?
    secrets << "ga_options:"
    secrets.concat(ga_options.lines.map { |line| "  #{line.rstrip}" })
  end

  File.write(output_path, "#{secrets.join("\n")}\n")

  validate_yaml!(output_path, fatal: false)

  puts "Wrote #{output_path}"
end

def validate_yaml!(output_path, fatal:)
  YAML.safe_load(File.read(output_path), permitted_classes: [], permitted_symbols: [], aliases: false)
rescue Psych::SyntaxError => e
  message = "rake warning: invalid YAML generated for #{output_path}: #{e.message}"

  fatal ? abort(message) : warn(message)
end

def validated_ga_measurement_id
  # GA4 measurement IDs normally follow the G-XXXXXXXX pattern.
  validate_variable_value(
    "GA_MEASUREMENT_ID",
    ENV["GA_MEASUREMENT_ID"],
    kind: :regex,
    pattern: /\AG-[A-Z0-9]+\z/i,
    on_invalid: :warn,
    required: false
  )
end

def validated_ga_options
  # Keep the raw multiline YAML so the workflow and local builds share the same shape.
  validate_variable_value(
    "GA_OPTIONS",
    ENV["GA_OPTIONS"]&.rstrip,
    kind: :yaml,
    on_invalid: :warn,
    required: false
  )
end

def validate_variable_value(name, value, kind:, pattern: nil, on_invalid:, required:)
  # Normalize only the whitespace that is safe for the validation mode in use.
  normalized_value = kind == :yaml ? value.to_s.rstrip : value.to_s.strip

  if normalized_value.empty?
    return nil unless required

    return report_invalid_value(name, "is required", on_invalid)
  end

  case kind
  when :regex
    regex = pattern.is_a?(Regexp) ? pattern : Regexp.new(pattern.to_s)
    return normalized_value if regex.match?(normalized_value)

    report_invalid_value(name, %(has invalid format: #{normalized_value.inspect}), on_invalid)
  when :yaml
    YAML.safe_load(normalized_value, permitted_classes: [], permitted_symbols: [], aliases: false)
    normalized_value
  else
    report_invalid_value(name, "uses unsupported validation kind #{kind.inspect}", on_invalid)
  end
rescue Psych::SyntaxError => e
  report_invalid_value(name, "contains invalid YAML: #{e.message}", on_invalid)
end

def report_invalid_value(name, message, on_invalid)
  full_message = "rake warning: ignoring invalid #{name} value: #{message}"

  case on_invalid
  when :abort
    abort(full_message)
  else
    warn(full_message)
    nil
  end
end

def digest_asset_source(source)
  if source.is_a?(Array)
    Digest::SHA256.hexdigest(source.map { |path| File.binread(path) }.join("\n"))
  else
    Digest::SHA256.file(source).hexdigest
  end
end
