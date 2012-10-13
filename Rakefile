# Encoding: utf-8
# ------------------------------------------------------------------------------
# Blog görevleri
# ------------------------------------------------------------------------------
require 'rubygems'

desc 'Begin a new post'
task :post do
	dizin = ARGV[1]
	title = ARGV[2]
	tags = ARGV[3]
	category = ARGV[4]
	unless dizin
		puts %{Usage: rake post 0 "The Post Title" "Tag 1, Tag 2" "Category"}
		exit(-1)
	end

	datetime = Time.now.strftime('%Y-%m-%d')  # 30 minutes from now.
	slug = title.strip.downcase.gsub(/ /, '-')

	# E.g. 2012-10-11-naber-haci-dayi.md
	path = "#{dizin}/_posts/#{datetime}-#{slug}.md"

	header = <<-END
---
title: #{title}
tag:  #{tags}
layout: post
category: #{category}
---

	END

	File.open(path, 'w') {|f| f << header }
	system("vim", path)
end
