# encoding: UTF-8

require "fileutils"
require "time"

include FileUtils

SOURCE = "source".freeze

CONFIG = {
  "posts" => File.join(SOURCE, "_posts"),
  "post_ext" => "md"
}.freeze

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
    post.puts tags_or_empty(categories)
    post.puts "tags:"
    post.puts tags_or_empty(tags)
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

  mkdir_p(File.dirname(filename))
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

  value.split(",").map { |item| "- #{item.strip}" }.join("\n")
end

def tags_or_empty(value)
  value.nil? || value.empty? ? "" : value
end