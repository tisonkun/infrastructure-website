Title: Manage your project web site

This page provides an overview of the tools Apache projects use to manage their own websites, and the tools Infra uses to manage the overall `apache.org` website. Everyone, even non-committers, can suggest changes to any Apache project website using these tools; committers on each project can use them to update their website very easily.

<h2>Contents</h2>

<ul>
<li><a href="#intro">Managing an Apache project's website</a></li>
<li><a href="#default">Is there a default website template?</a></li>
<li><a href="#preview">Previewing a project website</a></li>
<li><a href="#configure">Can I control the configuration of my project website?</a></li>
<li><a href="#logging">How does logging work?</a></li>
<li><a href="#which-tools-do-i-have-to-use">Which tools do I have to use?</a></li>
<li><a href="#svnpubsub-revision">What revision of the site is currently being served?</a></li>
<li><a href="#mail">How to provide public access to our project mail archive mbox files?</a></li>
<li><a href="#feather">Can my project site use its own favicon instead of just the Apache Feather?</a></li>
<li><a href="#generated">How can I minimize the number of changes committed in my Maven/JavaDoc/generated site?</a></li>
</ul>

<h2 id="intro">Managing an Apache project's website</h2>

Every Apache project or podling has a website hosted at `apache.org`. Apache provides tools to maintain it. Each project decides how their website looks, its contents, how they maintain it, and what software they use to support it, as long as the result is static files that our public web servers can make available to browsers. We also have limited support for .htaccess files and CGI scripts.

The basic requirements for site management are that 

  - only committers should be able to modify the site.
  - notifications of all site changes should be sent to the relevant project mailing lists. The Apache CMS allows non-committers to send  patches via a simple web interface by "Mail Diff"ing to the project  mailing list that can be easily published by a committer.

Infra supports these tools for publishing and maintaining Apache project websites:

  - **Apache CMS**, _(being deprecated; don't use for new sites)_ provides a simple browser-based user interface for editing, staging and publishing site content in Markdown, HTML or any other source format for which support has been added. See the <a href="cmsref">CMS reference</a> and <a href="cmsadoption">adoption</a> for more details. The Apache CMS uses pypubsub as the underlying mechanism for publishing a site.
  - **svnpubsub** automatically publishes the static contents of a designated svn folder (<a href="https://svn.apache.org/repos/asf/ant/site/ant/production/" target="_blank">example</a>) as the project web site at `http://project.apache.org`>. The project team can use any site build mechanism it wants as long as the above requirements are met.
  - **gitpubsub** automatically serves the static contents of a designated git as the website for a project. Git-based websites are typically maintained in a asf-site branch to be published as `https://project.apache.org`. A project can host the site from its primary project repo. Typically these will be built as a jenkins or a buildbot job. We recommend that you only have a single writer to the asf-site branch to avoid potential conflicts.
  
<h2 id="default">Is there a default website template?</h2>

Projects are free to choose their own styles and layout for websites. However if you'd like a quick way to get started with an easy-to-use, Markdown-based site that many other projects started with, see the <a href="https://github.com/apache/apache-website-template">Sample Apache Project website</a> repo, which has many useful features and instructions for using svnpubsub easily.

<h2 id="preview">Previewing a project website</h2>

  - For svnpubsub sites, review the local files in your svn checkout before committing them. The changes will be published immediately after you commit them.</p>
  - For CMS sites, just commit the changes (without "publish"-ing them) and browse to `>http://TLP.staging.apache.org/`. (For example, <a href="https://www.staging.apache.org/dev/project-site">the staging version of this page</a>.)  The CMS web interface includes a `Staged` link that will take you there directly.
  - There is no preview mode for gitpubsub. You should ideally have a way to locally build and test the website.
  
<h1 id="configure">Can I control the configuration of my project website?<a class="headerlink" href="#configure" title="Permanent link">&para;</a></h1>
<p>Yes, the central config file allows you to use <code>.htaccess</code> files in your
website directories to control configuration. Of course, reading and
parsing an <code>.htaccess</code> file on each request can slow down the server, so
you should consider requesting an adjustment in the central config file for
permanent configuration changes.</p>
<h1 id="logging">How does logging work?<a class="headerlink" href="#logging" title="Permanent link">&para;</a></h1>
<p>The build output from your job is available from either buildbot or jenkins, depending on which you use.</p>
<h1 id="which-tools-do-i-have-to-use">Which tools do I have to use?<a class="headerlink" href="#which-tools-do-i-have-to-use" title="Permanent link">&para;</a></h1>
<p>Projects can use one of the three options.  Note that if you use CMS you are using svnpubsub under the covers.</p>
<p>gitpubsub is based on git repos so if you're using that for source control it may make sense to use that over a svnpubsub directory.</p>
<h1 id="svnpubsub-revision">What revision of the site is currently being served?<a class="headerlink" href="#svnpubsub-revision" title="Permanent link">&para;</a></h1>
<p>This only applies to SVN based websites.</p>
<p>Look at the <code>.revision</code> file at the root of your site (for example,
<a href="http://subversion.apache.org/.revision">http://subversion.apache.org/.revision</a>).  That file is updated after every
successful <code>svn update</code>.  (If the update is underway or exited abnormally,
<code>.revision</code> won't have been changed.)</p>
<h1 id="mail">How to provide public access to our project mail archive mbox files?<a class="headerlink" href="#mail" title="Permanent link">&para;</a></h1>
<p>Some projects have a "mail" directory at the top of their project website.
Enable this by creating a symbolic link in svnpubsub or CMS output
(to <code>/home/apmail/public-arch/$tlp.apache.org</code>).
Also see other <a href="http://apache.org/dev/#mail">notes</a>.</p>
<h1 id="feather">Can my project site use its own favicon instead of just the Apache Feather?<a class="headerlink" href="#feather" title="Permanent link">&para;</a></h1>
<p>Yes, just add a <code>favicon.ico</code> file to your site's root.  The feather is only
used for project sites that don't have a <code>favicon.ico</code> file.</p>
<h1 id="generated">How can I minimize the number of changes committed in my Maven/JavaDoc/generated site?<a class="headerlink" href="#generated" title="Permanent link">&para;</a></h1>
<p>If you are using svnpubsub, the commit will perform very slowly if the number of changes is large - particularly if the number of files is large.
This is often the case with JavaDoc, and to a lesser extent Maven generated sites.</p>
<p>You may wish to use the CMS instead of svnpubsub directly. This moves the work to the server-side, but it will still be valuable to minimize the changes committed.</p>
<p>These are some steps that can be taken:</p>
<ul>
<li>Ask infrastructure to store your generated content in the CMS repository, rather than the main ASF repository. While this won't necessarily speed up the commit, it will prevent it from impacting other users.</li>
<li>When running JavaDoc, pass the <code>-notimestamp</code> option. This will avoid most files from being modified between runs if there haven't been code changes.</li>
<li>Use a Maven skin that doesn't generate a timestamp into the output. This may require customizing the current skins.</li>
<li>If you use the Maven dependencies report in the Project Info Reports plugin, use version 2.7+ of the plugin which avoids random strings being generated.</li>
<li>If you maintain historical versions of documentation, always check-in to a single "trunk", then use an <code>svn copy</code> operation to tag or branch the content, rather than checking in a complete copy</li>
<li>Minimize uses of "publish date" and "version" in templates to those that are truly necessary or helpful. Consider using a "last modified date" and version in the URL instead (unless "latest" is implied).</li>
<li>Minimize Subversion keywords in the output that may change frequently without significant meaning. This includes in source code that may be rendered to JavaDoc or a Maven JXR source cross-reference).</li>
<li>Avoid publishing Maven reports that change constantly to the project site. Code coverage, style reports, static analysis, etc. can be generated into a working copy on the CI server instead for easy developer viewing, and project's can consider utilizing https://analysis.apache.org/ for such reports as well.</li>
</ul></div>


_moving information here from https://www.apache.org/dev/project-site.html_