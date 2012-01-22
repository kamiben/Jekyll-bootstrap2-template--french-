task :default => :server

desc 'Clean up generated site'
task :clean do
  cleanup
end

desc 'Build site with Jekyll'
task :build => :clean do
  less
  jekyll
end

desc 'Start server with --auto'
task :server => :clean do
  less
  jekyll('--server --auto')
end

# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post") (credits : https://github.com/plusjade/jekyll-bootstrap/blob/master/Rakefile)
desc "Begin a new post in _posts"
task :new_post, :title do |t, args|
  abort("rake aborted: _posts directory not found.") unless FileTest.directory?("_posts")
  args.with_defaults(:title => 'Nouvel article')
  slug = args.title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  filename = File.join("_posts", "#{Time.now.strftime('%Y-%m-%d')}-#{slug}.md")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{args.title.gsub(/-/,' ')}\""
    post.puts "comments: true"
    post.puts "category: "
    post.puts "tags: []"
    post.puts "---"
  end
end # task :new_post

def ask(message, valid_options)
  if valid_options
    answer = get_stdin("#{message} #{valid_options.to_s.gsub(/"/, '').gsub(/, /,'/')} ") while !valid_options.include?(answer)
  else
    answer = get_stdin(message)
  end
  answer
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end

def cleanup
  sh 'rm -rf _site'
end

def jekyll(opts = '')
  sh 'jekyll ' + opts
end

def less(opts = '')
  sh 'cd _less && lessc styles.less > ../stylesheets/styles.css && cd ..'
end
