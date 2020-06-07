deploy_dir      = "/tmp/_deploy"   # deploy directory (for Github pages deployment)
public_dir      = "_site"    # compiled site directory
deploy_branch  = "gh-pages"

desc "deploy to github pages"
multitask :deploy do
  puts "## Deploying branch to Github Pages "
  (Dir["#{deploy_dir}/*"]).each { |f| rm_rf(f) }
  Rake::Task[:copydot].invoke(public_dir, deploy_dir)
  puts "\n## copying #{public_dir} to #{deploy_dir}"
  system "mkdir -p #{deploy_dir}"
  cp_r "#{public_dir}/.", deploy_dir
  system "git checkout #{deploy_branch}"
  system "cp -r #{deploy_dir}/* ."
  system "git add ."
  system "git add -u"
  puts "\n## Commiting: Site updated at #{Time.now.utc}"
  message = "Site updated at #{Time.now.utc}"
  system "git commit -m \"#{message}\""
  puts "\n## Pushing generated #{deploy_dir} website"
  system "git push origin #{deploy_branch} --force"
  system "git checkout master"
  puts "\n## Github Pages deploy complete"
end
