---
author: Kevin
categories:
- Uncategorized
date: 2007-12-12T12:59:19Z
guid: http://cleverswine.net/?p=276
id: 277
tags:
- Hacking
title: ActiveRecord::Migration
url: /2007/12/12/activerecordmigration/
---

I started a new Ruby on Rails project today, and I decided to create the database using migrations. I should have been doing this all along, but I never really understood migrations or what the benefit might be. Well, now I know. In about 10 minutes, I created my Rails migration files, and then I ran &#8220;rake db:migrate&#8221;. Bam! There was my MySQL database, just as I had planned for it to look. Amazing! To see the simplicity first hand, here is a code sample using Rails 2.0.
  
`</p>
<pre>
class Tasks < ActiveRecord::Migration
  def self.up
    create_table :tasks do |t|
      t.references :section, :project, :status
      t.string :name, :null => false
      t.integer :hrs
      t.timestamps
    end
  end
end
</pre>
<p>`

The resulting table is:
  
``</p>
<pre>
CREATE TABLE `tasks` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `section_id` int(11) DEFAULT NULL,
  `project_id` int(11) DEFAULT NULL,
  `status_id` int(11) DEFAULT NULL,
  `name` varchar(255) NOT NULL,
  `hrs` int(11) DEFAULT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
)
</pre>
<p>``