---
layout: post
title: "Debug Drupal PHP in Vim with Vdebug"
date: 2017-04-19 12:00
author: Bob Kepford
comments: false
categories:
- Vim
- PHP
- Drupal
- Debugging
---

<p>Republished from <a href="https://www.mediacurrent.com/blog/debug-drupal-php-vim-vdebug">Mediacurrent.com</a></p>
<p>I know quite a few developers that love Vim but think they have to use an IDE to be able to debug their applications. In this post I will show you how to set up Vim as a Xdebug client.</p>
<p>The <a href="https://github.com/joonty/vdebug" target="_blank">Vdebug plugin</a> for Vim provides a way to do debugging inside Vim using the tools that Vim provides to great advantage. As the project page says,</p>
<p>&nbsp;</p>
<blockquote><p>Vdebug is a new, fast, powerful debugger client for Vim. It's multi-language, and has been tested with PHP, Python, Ruby, Perl, Tcl and NodeJS. It interfaces with any debugger that faithfully uses the DBGP protocol, such as Xdebug for PHP.</p></blockquote>
<p>&nbsp;</p>
<p>In this post we will focus on PHP, and more specifically, Drupal.</p>
<h2>Xdebug</h2>
<p>The first step we must take is to confirm you have Xdebug working in your application. If you don't have Xdebug installed head over to their site and follow the <a href="https://xdebug.org/docs/install" target="_blank">installation guide</a>. You don't have to do anything special in order to use Vdebug with your PHP application. <strong><span class="inline-comment-marker" data-ref="a75ecee3-7e7d-4c90-abab-6c96809eb873">If you are currently using another debugging client like the one built into PHPStorm you shouldn't have to change anything in your PHP configuration</span>.</strong> But, for sake of completeness here is an example Xdebug configuration we (Mediacurrent) use on all of our projects. We have standardized our local development work on the great <a href="https://www.drupalvm.com/" target="_blank">Drupal VM</a> project.</p>
<pre><code>
[XDebug]
zend_extension="/usr/lib/php5/modules/xdebug.so"
xdebug.coverage_enable=0
xdebug.default_enable=0
xdebug.remote_enable=1
xdebug.remote_connect_back=1
xdebug.remote_host=10.0.2.2
xdebug.remote_port=9000
xdebug.remote_handler=dbgp
xdebug.remote_log=/tmp/xdebug.log
xdebug.remote_autostart=false
xdebug.idekey="PHPSTORM"
xdebug.max_nesting_level=256</code></pre><h2>Xdebug Helper for Chrome</h2>
<p>The next step is to set up your browser to allow you to easily enable and disable Xdebug. To do this, I use the <a href="https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc?hl=en" target="_blank">Xdebug helper</a> for Chrome. It's much easier than setting up POST variables and cookies for each browser session. You will want to make sure your `<span class="inline-comment-marker" data-ref="a6b23600-8fcd-4caa-bee4-2a21dc75f341">idekey`</span> is set to match what you are using in xdebug on the server. <span class="inline-comment-marker" data-ref="eccf67a3-b18c-4ac3-8f16-84e7cadd71e4">In my case,</span> that is PHPSTORM.</p>
<h2>Vdebug Features</h2>
<p>Now that we have Xdebug all squared <span class="inline-comment-marker" data-ref="e6d8e382-9cb2-401e-8bc1-0a85dd07f285">away,</span> <span class="inline-comment-marker" data-ref="f25676ed-4c1d-4e9f-8a75-55ad0b2c4881">let's</span> learn about Vdebug. Just what can Vdebug allow you to do?</p>
<ul>
<li>Set and toggle <span class="inline-comment-marker" data-ref="567b3254-8d43-4879-8bb6-1c3598f08846">breakpoints.</span></li>
<li>Step through your code line by line.</li>
<li>Step over functions and methods.</li>
<li>Step into and out of functions and methods.</li>
<li>Run the application to the cursor.</li>
<li>Evaluate variables under the cursor at run time.</li>
<li>Navigate objects and arrays.</li>
<li>Evaluate code and display the result.</li>
</ul>
<h2>Installing Vdebug</h2>
<p>If you don't already use a plugin manager for Vim I recommend you check out <a href="https://github.com/VundleVim/Vundle.vim" target="_blank">Vundle</a>, but the way you install Vdebug has no affect on how you use it. I recommend that you follow the <a href="https://github.com/joonty/vdebug#installation" target="_blank">installation instructions</a> on the Vdebug project page to get it set up.</p>
<h3>Default Keybindings</h3>
<p><span>For your convenience, I have listed the default key bindings for Vdebug below (I have changed the defaults in my own vimrc since I find them to be somewhat difficult to remember).</span></p>
<pre><code>&lt;F5&gt;: start/run (to next breakpoint/end of script)
&lt;F2&gt;: step over
&lt;F3&gt;: step into
&lt;F4&gt;: step out
&lt;F6&gt;: stop debugging (kills script)
&lt;F7&gt;: detach script from debugger
&lt;F9&gt;: run to cursor
&lt;F10&gt;: toggle line breakpoint
&lt;F11&gt;: show context variables (e.g. after "eval")
&lt;F12&gt;: evaluate variable under cursor
:Breakpoint &lt;type&gt; &lt;args&gt;: set a breakpoint of any type (see :help VdebugBreakpoints)
:VdebugEval &lt;code&gt;: evaluate some code and display the result
&lt;Leader&gt;e: evaluate the expression under visual highlight and display the result</code></pre><h2>Using Vdebug</h2>
<p>Note: during this screen cast I am using my own key bindings which I outline later in this post.</p>
<div style="position:relative;height:0;padding-bottom:56.25%">
<div class="fluid-width-video-wrapper"><iframe allowfullscreen="" frameborder="0" src="https://www.youtube.com/embed/_6LV_XgP6Fs?rel=0?ecver=2" style="position:absolute;width:100%;height:100%;left:0" id="fitvid202558"></iframe></div></div>
<p><a href="https://www.youtube.com/watch?v=_6LV_XgP6Fs" target="_blank">Screen cast</a></p>
<h2>Debugging Drupal in a Virtual Machine</h2>
<p>If you only ever debug using your machine's native version of PHP you can skip this step. But if you need to connect to a server running your <span class="inline-comment-marker" data-ref="4823cb92-8129-4f66-97ed-e985c7f7e146">code (Vagrant</span> or <span class="inline-comment-marker" data-ref="5f06276a-e8c1-4a26-b74f-34eaaa3b011e">Remote),</span> you will need to do one more step to set up Vdebug. You need to tell Vdebug where your files are on your local file system and the server's files system. The reason you need to do this is because the file paths on your local machine and the server are most likely different. Let's look at an example.</p>
<pre><code>" Mapping '/remote/path' : '/local/path'
let g:vdebug_options['path_maps'] = {
      \  '/var/www/drupalvm/web' : '/Users/kepford/Sites/someproject<span class="inline-comment-marker" data-ref="ff88e852-8453-4a3b-bb4f-07f73050b2f7">/web',</span>
      \  '/home/vagrant/docroot' : '/Users/kepford/Sites/anotherproject/docroot'
      \}</code></pre><p>The first portion of the mapping is for the remote path <code>/var/www/drupalvm/web</code>. The second is for the local path <code>/Users/kepford/Sites/someproject/web</code>. That's all you have to do for remote debugging.</p>
<h2>Customizing Vdebug</h2>
<p>Now that we have everything set up let's talk about custom configuration. The default keybindings that ship with Vdebug may not suit your preferences. Personally, I try to come up with mnemonics for each Vim command and struggle to remember the correct function keys for Vdebug. The good news is it's easy to override the defaults in your .vimrc file. Here's an example of what I have done with my defaults.</p>
<pre><code>let g:vdebug_keymap = {
\    "run" : "&lt;Leader&gt;/",
\    "run_to_cursor" : "&lt;Down&gt;",
\    "step_over" : "&lt;Up&gt;",
\    "step_into" : "&lt;Left&gt;",
\    "step_out" : "&lt;Right&gt;",
\    "close" : "q",
\    "detach" : "&lt;F7&gt;",
\    "set_breakpoint" : "&lt;Leader&gt;s",
\    "eval_visual" : "&lt;Leader&gt;e"
\}</code></pre><p><em>Note: I have my <code>&lt;Leader&gt;</code> key set to be the space bar and find this is very productive.</em></p>
<p>You may not like my key mappings but they work for me. The rest of my configuration is fairly straight forward.</p>
<pre><code>" Allows Vdebug to bind to all interfaces.
let g:vdebug_options = {}

" Stops execution at the first line.
let g:vdebug_options['break_on_open'] = 1
let g:vdebug_options['max_children'] = 128

" Use the compact window layout.
let g:vdebug_options['watch_window_style'] = 'compact'

" Because it's the company default.
let g:vdebug_options['ide_key'] = 'PHPSTORM'


" Need to set as empty for this to work with Vagrant boxes.
let g:vdebug_options['server'] = ""</code></pre><h2>Debugging the Debugger</h2>
<p>If you encounter a problem getting Vdebug to work it's likely that you need to change a setting. But before you do that you should turn on debugging. Run the follow two commands to dump the output of the Vdebug log to a file.</p>
<pre><code>:VdebugOpt debug_file ~/vdebug.log
:VdebugOpt debug_file_level 2</code></pre><p>Then tail the output of this file like so, <code>tail -f ~/vdebug.log</code> as you start Vdebug and try to use it.</p>
<h2>More reading</h2>
<p>As with any good Vim plugin you can learn a lot more about how to use it by reading the help documentation at <code>:help Vdebug</code>.</p>


