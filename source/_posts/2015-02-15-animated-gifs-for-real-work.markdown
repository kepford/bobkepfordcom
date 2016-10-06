---
title: "Animated Gifs for Real Work"
categories:
- Productivity
- Apps
- Gifs
- Screencasting
---
<p>Republished from <a href="https://www.mediacurrent.com/blog/animated-gifs-real-work">Mediacurrent.com</a></p>
<p><strong>Amaze your friends and co-workers with your cool animated gif screencasts!</strong></p>
<p>A picture is worth a thousand words. It’s a tired cliché but it’s generally true. Sometimes it’s easier to show someone a thing than it is to explain that thing using the written word. Working with a distributed team has it’s share of challenges and one is explaining how to reproduce a bug or how a new feature should work. What if you could quickly record a video and paste it into a your ticketing system? And by quickly I mean seconds, not minutes.</p>
<p> <img src="https://cl.ly/1S450e1w1c08/Image%202016-10-05%20at%209.56.16%20PM.gif" alt="Woody"/></p>
<p>We are all familiar with animated gifs. They’re a lot of fun. They can also be useful when doing dev and design work. This tutorial will show you how to create an animated gif screencast. Let’s begin by breaking this down into 3 steps.</p>
<ol>
<li>Create the gif</li>
<li>Upload the gif to the web</li>
<li>Share your gif!</li>
</ol>
<h2>Create the Gif</h2>
<p>Most tools for creating animated gifs are designed to convert a movie snippet into a gif. We want to capture our screen as an animated gif. The best tool I’ve found for doing is an app called <a href="http://www.cockos.com/licecap/">LICEcap</a>. Horrible name but it’s a great free and open source (Mac / Windows) app that is easy to use.</p>
<p> <img src="https://cl.ly/2N1X3S3F1Q1M/Image%202016-10-05%20at%209.57.56%20PM.gif" alt="Gif example" /></p>
<h2>Upload the gif to the web</h2>
<p>Once we have our gif we want to be able to quickly share it. The goal is to quickly upload the file and copy a publicly shareable URL. What I’ve done is set up a directory on my own server to store my gifs but you could use any number of file sharing services. I recommend<a href="http://getcloudapp.com/"> Cloud App</a> or<a href="https://www.dropbox.com/"> Dropbox</a>.</p>
<p>If you are a Mac user there is a great little app called<a href="http://www.noodlesoft.com/hazel"> Hazel</a> (paid app) that does many cool things. At it’s core it’s a<a href="http://en.wikipedia.org/wiki/Daemon_(computing)"> daemon</a> that waits for triggers to launch it into action. It’s like a little digital worker on your computer. My favorite thing that is does is watch a directory for changes. When a new file hits a watched directory it jumps into action and does whatever you set it to do. In this case I want Hazel to execute a script that uploads the Gif file to my personal server. This script then copies the gif URL to my clipboard and then moves this file to a folder named uploaded. So, all that I have to do is move the file to a folder named Gifs and within seconds I have a shareable URL on the clipboard.</p>
<p> <img src="https://cl.ly/0U2V3l332c39/Image%202016-10-05%20at%209.59.06%20PM.png" alt="Hazel"/></p>
<p>This simple shell script copies the URL to the clipboard.</p>
<p><code>x="$1" y=${x%}</code><br><code>echo http://mydomain.com/${y##*/} | pbcopy</code></p>
<h2>Share your gif!</h2>
<p>Now that I have a URL for the gif on my clipboard I’m ready to share it. Where I am sharing the gif will determine how I proceed. If the system supports the image tag I use a<a href="http://smilesoftware.com/TextExpander"> TextExpander</a> snippet to quickly drop an image tag into my comment body. So the next time you have to explain a complex bit of functionality, why not spend that time making an animated gif that demonstrates what you are trying to explain.</p>
<p> <img src="https://cl.ly/160W281b1L0u/Image%202016-10-05%20at%2010.00.01%20PM.gif" alt="Thumbs up"/></p>
<p><strong>Additional apps to consider</strong></p>
<ul>
<li>
<p>Add mouse and keyboard effects to your Gifs and screencasts with<a href="http://boinx.com/mousepose/overview/"> Mousepose</a>.</p>
</li>
<li>
<p>Drag and drop your Gifs to easily upload them many places with<a href="http://aptonic.com/dropzone/"> Dropzone</a>.</p>
</li>
</ul>
