require "rake"
require "rake/clean"

task default: :test

dizzy_base = "2007-02-27-making-dizzy-shine-with-ajax"
dizzy_adoc = "_posts/_#{dizzy_base}.asciidoc"
dizzy = "_posts/#{dizzy_base}.html"
CLEAN.include FileList[dizzy, "_site"]

file dizzy => dizzy_adoc do
  require "asciidoctor"
  options = { attributes: { "skip-front-matter" => true } }
  doc = Asciidoctor.load_file(dizzy_adoc, options)
  File.open(dizzy, "w") do |f|
    f.write <<~YAML
      ---
      #{doc.attributes["front-matter"]}
      ---
    YAML
    f.puts doc.render
  end
end

task deps: dizzy

task jekyll: :deps do
  sh "jekyll", "build"
end

desc "Run html proofer to validate the HTML output."
task test: :jekyll do
  require "html-proofer"
  HTMLProofer.check_directory(
    "./_site",
    check_external_hash: true,
    check_favicon: true,
    check_opengraph: true,
    check_html: true,
    check_img_http: true,
    parallel: { in_processes: 4 },
    url_ignore: [
      %r{^https://web\.archive\.org/web/},
    ]
  ).run
end
