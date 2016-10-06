---
title: "Efficient Drupal Development with Tmux and Tmuxinator"
categories:
- Commandline
- Tmux
- Drush
- Drupal
---
<p>Republished from <a href="https://www.mediacurrent.com/blog/efficient-drupal-development-tmux-and-tmuxinator">Mediacurrent.com</a></p>
<p><span style="line-height: 1.538em;">Have you ever wished you could just type one command and load up all of the things you need to work on for a project? Wouldn&rsquo;t it be nice to have your terminal set up with the correct</span><a href="https://www.mediacurrent.com/blog/make-your-drush-aliases-work-local-and-remote" style="line-height: 1.538em;"> Drush alias</a><span style="line-height: 1.538em;">, tailing the watchdog, with access to your servers just a couple keystrokes away? Sounds nice, right? With a little work and a handful of open source applications this can be a reality for you.</span></p><p>When I need to get started on one of my projects it&rsquo;s as simple as typing mux projectname and I pick up right where I left off the previous day. Here&rsquo;s how I do it.</p><h2><a href="http://tmux.sourceforge.net/">Tmux</a></h2><p>The cornerstone application that I use every day is a terminal multiplexer called Tmux. It allows you to quickly switch between multiple terminal programs using key commands. One on my favorite features of tmux are the multi-pane windows. You can divvy up your terminal window into two, three, or more panes. Each one running it&rsquo;s own terminal session. While this is useful enough, its true utility is its ability to detach from a session without interrupting or canceling it. For example, let&rsquo;s say you are downloading a large file with wget on a server and want to check something else on your local machine while you wait. You can do so without opening a new terminal window by detaching from your tmux session. Your download will continue in the background. Then, when you are ready to resume work on your server simply re-attach to your tmux session.</p><p>That all sounds great, but how can this application help me with my local development work? This is where<a href="https://github.com/tmuxinator/tmuxinator"> Tmuxinator</a> comes in. Tmuxinator provides a method for creating and managing tmux sessions. I can&rsquo;t imaging using tmux without it. I have a default template that I begin with on every project. It includes my terminal layouts, commands I need to run when beginning work, and windows for development, staging, and production servers, and setting up each pane with the proper<a href="https://github.com/drush-ops/drush"> Drush</a> or bash alias.</p><h2>Installation</h2><h3><a href="http://tmux.sourceforge.net/">Tmux</a></h3><p>On a Mac you can quickly install Tmux using<a href="http://brew.sh/"> Homebrew</a>.</p><p>&nbsp;</p>
<pre>
brew install tmux</pre>

<p>&nbsp;</p>
<p>Once you have tmux installed you will want to take a look at the post on the Thoughtbot&rsquo;s blog &ldquo;<a href="http://robots.thoughtbot.com/a-tmux-crash-course">A tmux Crash Course</a>&rdquo; and set up your own keybinding configuration. Then you will want to set up copy and paste to work as it does in<a href="http://www.vim.org/"> Vim</a> by following this<a href="http://robots.thoughtbot.com/tmux-copy-paste-on-os-x-a-better-future"> Thoughtbot </a><a href="http://robots.thoughtbot.com/tmux-copy-paste-on-os-x-a-better-future">tutorial</a>. If you are on Linux you may find that you don&rsquo;t need some of these configuration changes. Specifically the copy and paste settings. I have included my .tmux.conf file below. Note, this file is located in your home directory and is a dot file so it will be hidden in most file browsers.</p>
<p><strong>My .tmux.conf file</strong></p>

<pre>
# Use vi mode

setw -g mode-keys vi

# fix pbcopy and pbpaste

set-option -g default-command &quot;reattach-to-user-namespace -l bash&quot;

# Setup &#39;v&#39; to begin selection as in Vim

bind-key -t vi-copy v begin-selection

bind-key -t vi-copy y copy-pipe &quot;reattach-to-user-namespace pbcopy&quot;

# Update default binding of `Enter` to also use copy-pipe

unbind -t vi-copy Enter

bind-key -t vi-copy Enter copy-pipe &quot;reattach-to-user-namespace pbcopy&quot;

# force a reload of the config file

unbind r

bind r source-file ~/.tmux.conf

# mouse scrolling and selection

set -g mode-mouse on

set -g default-terminal &quot;screen-256color&quot;
</pre>

<h3><a href="https://github.com/tmuxinator/tmuxinator">Tmuxinator</a></h3>
<p>Now you should be ready to use Tmux, but let&rsquo;s also set up Tmuxinator. Tmuxinator is a Ruby gem and can be installed with the command <code>gem install tmuxinator</code> but do follow the instructions on the<a href="https://github.com/tmuxinator/tmuxinator"> project page</a>.</p>
<h2>Setting up your first project</h2>
<p>Now, let&rsquo;s set up a new Tmuxinator project by typing mux new awesome_project. This will open up your project file in your default text editor.</p>

<pre>
# ~/.tmuxinator/awesome_project.yml

  name: awesome_project

  root: ~/

  # Optional tmux socket

  # socket_name: foo

  # Runs before everything. Use it to start daemons etc.

  # pre: sudo /etc/rc.d/mysqld start

  # Runs in each window and pane before window/pane specific commands. Useful for setting up interpreter versions.

  # pre_window: rbenv shell 2.0.0-p247

  # Pass command line options to tmux. Useful for specifying a different tmux.conf.

  # tmux_options: -f ~/.tmux.mac.conf

  # Change the command to call tmux.  This can be used by derivatives/wrappers like byobu.

  # tmux_command: byobu

  windows:

    - editor:

        layout: main-vertical

        panes:

          - vim

          - guard

    - server: bundle exec rails s

    - logs: tail -f log/development.log
</pre>

<p>Each tab in tmux is called a window. You can assign names to these windows which helps you remember what a window is used for. Inside each tmux window is one or more panes. The default configuration that Tmuxinator provides is very Ruby on Rails specific so let&rsquo;s change that. Here is a more Drupal specific tmuxinator project setup.</p>

<pre>
# ~/.tmuxinator/awesome_project.yml

# you can make as many tabs as you wish&hellip;

project_name: awesome_project

project_root: ~/Sites/awesome_project

windows:

    - drupal:

        layout: main-vertical

        panes:

        - cd docroot &amp;&amp; pstorm . &amp;&amp; drush use @awesome.mcdev &amp;&amp; git status

        - cd docroot &amp;&amp; drush use @awesome.mcdev &amp;&amp; drush ws --tail

        - cd mis_vagrant &amp;&amp; vagrant up

    - tig: cd docroot &amp;&amp; tig

    - dev: alias drush=&quot;/usr/local/bin/drush/drush&quot; &amp;&amp; drush use @drupalplatformprovider.awesome.dev

    - staging: alias drush=&quot;/usr/local/bin/drush/drush&quot; &amp;&amp; drush use @drupalplatformprovider.awesome.test

    - prod:  alias drush=&quot;/usr/local/bin/drush/drush&quot; &amp;&amp; drush use @drupalplatformprovider.awesome.live
</pre>

<p>Once you have made your changes save and close the project file. Type mux awesome_project to begin you first tmux session.</p>
<p> <img src="https://cl.ly/3J3u3A1U2f2J/Image%202016-10-05%20at%209.49.25%20PM.png" alt="Tmux" /></p>
<p>You will notice that I have several windows. Specifically, I want to point out the dev/staging/prod windows. I like to use<a href="https://www.mediacurrent.com/blog/make-your-drush-aliases-work-local-and-remote"> drush aliases</a> and with Tmuxinator you can execute terminal commands when you launch the session. Let me walk through what the tmuxinator project file is actually doing.</p>
<h3>Drupal Window</h3>
<ul><li>Pane 1: I cd to docroot and open that directory in PHPStorm. You can substitute this command for your editor of choice.</li><li>Pane 2: I cd to docroot and set drush to use my local alias then tail the drupal watchdog with drush use @awesome.mcdev &amp;&amp; drush ws &ndash;tail.</li><li>Pane 3: launches my vagrant box for this project</li></ul>
<h3>Tig Window</h3>
<p>I cd to docroot and execute the <a href="https://github.com/jonas/tig">Tig</a> application which shows me a list of commits to the repository in the command line.</p>
<h3>Dev / Stage / Prod</h3>
<p>On many projects our clients may be using a Drupal platform service such as Pantheon or Acquia Cloud. Some server environments may require the use of older versions of Drush than say Drush 7. I will have Tmuxinator set up each terminal window with the proper shell alias to my desired drush version and also the drush alias for that server.</p>
<h2>Is it worth it?</h2>
<p>While this may seem like quite a bit of work to set up the efficiency you can achieve with this workflow will quickly become apparent.</p>
<p><strong>Additional Resources</strong></p>
<p><a href="http://www.mediacurrent.com/blog/how-to-improve-output-from-remote-drush-commands" target="_blank">How To Prettify the Output From Your Remote Drush Commands</a> | Mediacurrent Blog Post<br><a href="http://www.mediacurrent.com/blog/learning-how-install-drush-non-admin-rights-server" style="line-height: 1.538em;" target="_blank">Learning How To Install Drush on Non-Admin Rights Server</a><span style="line-height: 1.538em;">&nbsp;| Mediacurrent Blog Post</span><br><a href="http://www.mediacurrent.com/blog/better-access-denied-403-page-panels" style="line-height: 1.538em;" target="_blank">A Better Access Denied Page with Panels</a><span style="line-height: 1.538em;"> | Mediacurrent Blog Post&nbsp;</span></p>
