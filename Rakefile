$post_ext ||= ".markdown"
$post_dir ||= "_drafts/"
$date_format ||= "%Y-%m-%d"

# Based off of the taks in the jekyll-rakefile project
#https://github.com/avillafiorita/jekyll-rakefile/blob/master/Rakefile
desc 'Start a draft'
task :create_draft, [:title] do |t, args|
  if args.title == nil then
    puts "Error! title is empty"
    puts "Usage: create_post[title]"
    puts "DATE is optional"
    puts "DATE is in the form: YYYY-MM-DD; use nil or empty for today's date"
    exit 1
  end

  post_title = args.title

  def slugify(title)
    # strip characters and whitespace to create valid filenames, also lowercase
    return title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  end

  filename = slugify(post_title) + $post_ext

  # generate a unique filename appending a number
  i = 1
  while File.exists?($post_dir + filename) do
    filename = File.basename(slugify(post_title)) + "-" + i.to_s + $post_ext 
    i += 1
  end

  # the condition is not really necessary anymore (since the previous
  # loop ensures the file does not exist)
  File.open($post_dir + filename, 'w') do |f|
    f.puts "---"
    f.puts "title: \"#{post_title}\""
    f.puts "layout: post"
    f.puts "---"
  end  

  puts "Post created under \"#{$post_dir}#{filename}\""
end

desc "Dev Server with Drafts"
task :devserver do |t|
    sh "bundle exec jekyll serve --drafts --host=0.0.0.0"
end
