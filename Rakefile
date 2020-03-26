# https://github.com/kippt/jekyll-incorporated/blob/master/Rakefile
# https://github.com/id-ruby/id-ruby/blob/f785e2e58a759165e3e592c7b27975a0a832ff34/Rakefile#L37

require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"

# Change your GitHub reponame 
# eg. "kippt/jekyll-incorporated"
# or "id-ruby/id-ruby"
GITHUB_REPONAME = "epsi-rns/workflows-jekyll"
GITHUB_TOKEN    = ENV["GITHUB_TOKEN"]


namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "_site"
    })).process
  end


  desc "Generate and publish blog to gh-pages"
  task :publish => [:generate] do
    current_directory = Dir.pwd

    Dir.mktmpdir do |tmp|
      cp_r "_site/.", tmp
      Dir.chdir tmp
      system "git init"
      system "git config user.name someone"
      system "git config user.email someone@somewhere"
      system "git add ."
      message = "Site updated at #{Time.now.utc}"
      system "git commit -m #{message.inspect}"
      system "git remote add origin #{git_origin}"
      system "git push origin master:refs/heads/gh-pages --force"
      Dir.chdir current_directory
    end
  end

  def git_origin
    if GITHUB_TOKEN
      "https://x-access-token:#{GITHUB_TOKEN}@github.com/#{GITHUB_REPONAME}.git"
    else
      "git@github.com:#{GITHUB_REPONAME}.git"
    end
  end
end
