$post_ext ||= ".markdown"
$post_dir ||= "_drafts/"
$date_format ||= "%Y-%m-%d"

# Based off of the taks in the jekyll-rakefile project
#https://github.com/avillafiorita/jekyll-rakefile/blob/master/Rakefile
desc 'Create a post'
task :create_post, [:title, :date] do |t, args|
  if args.title == nil then
    puts "Error! title is empty"
    puts "Usage: create_post[date,title,category,content]"
    puts "DATE is optional"
    puts "DATE is in the form: YYYY-MM-DD; use nil or empty for today's date"
    exit 1
  end

  if (args.date != nil and args.date != "nil" and args.date != "" and args.date.match(/[0-9]+-[0-9]+-[0-9]+/) == nil) then
    puts "Error: date not understood"
    puts "Usage: create_post[date,title,category,content]"
    puts "DATE is optional"
    puts "DATE is in the form: YYYY-MM-DD; use nil or the empty string for today's date"
    puts ""

    puts "Examples: create_post[\"\",\"#{args.title}\"]"
    puts "          create_post[nil,\"#{args.title}\"]"
    puts "          create_post[,\"#{args.title}\"]"
    puts "          create_post[#{Time.new.strftime("%Y-%m-%d")},\"#{args.title}\"]"
    exit 1
  end
  post_title = args.title
  post_date = (args.date != nil and args.date != "" and args.date != "nil") ? args.date : Time.new.strftime($date_format)

  def slugify(title)
    # strip characters and whitespace to create valid filenames, also lowercase
    return title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  end

  filename = post_date + "-" + slugify(post_title) + $post_ext

  # generate a unique filename appending a number
  i = 1
  while File.exists?($post_dir + filename) do
    filename = post_date + "-" +
      File.basename(slugify(post_title)) + "-" + i.to_s +
      $post_ext 
    i += 1
  end

  # the condition is not really necessary anymore (since the previous
  # loop ensures the file does not exist)
  File.open($post_dir + filename, 'w') do |f|
    f.puts "---"
    f.puts "title: \"#{post_title}\""
    f.puts "layout: post"
    f.puts "date: #{post_date}"
    f.puts "---"
  end  

  puts "Post created under \"#{$post_dir}#{filename}\""
end

desc "Dev Server with Drafts"
task :devserver do |t|
    sh "bundle exec jekyll serve --drafts --host=0.0.0.0"
end
