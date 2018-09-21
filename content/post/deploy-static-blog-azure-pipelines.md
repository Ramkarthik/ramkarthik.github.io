---
title: "How to Deploy Hugo Static Website Using Azure Pipelines"
date: 2018-09-20T10:21:52-07:00
draft: false
---

<p>This blog is generated using Hugo which is a static website generator written in Go. I host this on a Ubuntu VM.</p>
<p><h4>Steps I previously used to follow to publish a blog post</h4></p>
<ol>
	<li>Write the post in Markdown or HTML.</li>
	<li>Push to GitHub to make sure I don't ever lose anything.</li>
	<li>Generate blog by running 'hugo -b https://kramkarthik.com' and copy files in 'public' folder to the remote machine using SFTP.</li>
</ol>

<p>If you've done step three before, you will know that it is a boring and time consuming task. Also, it is the step where I might actually make a mistake.</p>

Now with Azure Pipelines, you can automate pretty much any deployment process.

<p><h4>After integrating with Azure Pipelines, this is my current process to publish a blog post:</h4></p>
<ol>
	<li>Write the post in Markdown or HTML</li>
	<li>Push to Github to make sure I don't ever lose anything.</li>
</ol>

<p>There is no step three. Azure Pipelines takes care of this for me.</p>

<p><h4>Here's how you can deploy your Hugo static website using Azure Pipelines</h4></p>
<span>Note: This tutorial assumes that you have your static Hugo website code hosted in a Git repository.</span>
<p></p>
<ol>
	<p><li>Go to <a href="https://dev.azure.com" target="_blank">https://dev.azure.com</a> and click on 'Create project'.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/create_azure_project.png" />
	<p><li>Click on 'Pipelines' and then in the next screen, click on 'New pipeline'. You will also have a 'Pipelines' option on the left menu.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/new_azure_project_landing.png" />
	<p><li>Choose the source where your code is hosted. For this tutorial, I'm using GitHub. Other providers should be quite similar. Give a connection name and click on Authorize using OAuth.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/connect_repository.png" />
	<p><li>Click on Authorize AzurePipelines.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/github_oauth.png" />
	<p><li>Choose your website repository and the branch. I want only commits or merge into 'master' branch to create a build and deploy, so I have chosen 'master' as branch. Click on continue.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/choose_repository.png" />
	<p><li>In the next screen, you will be asked to choose a template. Scroll all the way down to the end and you will find 'Empty Pipeline'. Let's choose that one.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/choose_empty_pipeline.png" />
	<p><li>Now we are building a pipeline. You can click on 'Agent Job 1' under pipeline and rename it. Once you rename it, click on the '+' sign.</li></p>
	<img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/build_pipeline.png" />
	<p><li>You will see a new tab to add a task on the right side. Select 'Marketplace', search for 'Hugo' and click on 'Get it free'. This will install the plugin. Once it is done, click on 'Add'. It will add a new 'Hugo generate' task to your job agent. Click on it.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/marketplace.png" /></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/marketplace_hugo.png" /></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/hugo_generate_task_pipeline.png" /></p>
	<p><li>This is where we will be configuring Hugo settings.
		<ul>
			<li>Enter destination as '$(Build.ArtifactStagingDirectory)/publish'.</li>
			<li>Base url will be the base url of your website. In my case, https://kramkarthik.com.</li>
			<p>The other options are not mandatory. You can choose based on your preference. I'm leaving them unchecked.</p>
			<p>Click the down arrow next to 'Save & queue' and click on 'Save'.</p>
			<p><b>Warning: Clicking on 'Save & queue' will trigger a build. We don't need a build at this point of time.</b></p>
		</ul>
	</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/hugo_task_settings.png" /></p>
	<p><li>Let's add another task. Click on the '+' on your job agent like you did to create the previous task. Now search for 'Publish Build Artifacts' and click on 'Add'. Once it is added, click on it to configure.</li></p>
	<p><li>Enter the details like below. Click on the down arrow next to 'Save & queue' and click on 'Save & queue'. This will trigger a manual build. We can confirm if we did things right so far.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/publish_artifact.png" /></p>
	<p><li>Once you run the build, you will see a screen similar to what you see below. You can confirm whether your 'public' contents got uploaded by clicking on Artifacts.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/initial_build.png" /></p>
	<p><li>Now we want to deploy this artifact to a remote machine. This will be part of 'Release'. So click on 'Releases' on the left side menu and then click on '+ New pipeline'.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/create_new_release.png" /></p>
	<p><li>Click on '+ Add' next to 'Artifacts' and then choose the source which you create in previous step from the dropdown.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/add_artifact.png" /></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/choose_artifact.png" /></p>
	<p><li>Now click on '1 job, 0 task' under 'Stage 1', rename it to 'DeployBlog' and add a new task from the agent job. Search for 'Copy files over SSH' and add it to your agent job.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/create_copy_ssh.png" /></p>
	<p><li>Time to configure SSH. Before we add a new SSH service connection, let's choose the source folder. Click on the browse icon (three dots) next to 'Source Folder' and choose the 'blog' folder in the artifact that you created in the build step.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/configure_copy_files_settings.png" /></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/choose_deploy_artifact.png" /></p>
	<p><li>Now we will configure SSH service connection. Click on 'Manage' and it will open a new tab where you can add a new service connection. Click on 'New service connection', scoll down (there won't be a visible scrollbar but you can scroll and then you will see it) and choose 'SSH'.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/new_service_connection.png" /></p>
	<p><li>Enter the following to create a new SSH service connection:
		<ul>
			<li>Connection Name: [Some meaningful name you can identify this connection with].</li>
			<li>Host name: mostly your domain name (in my case: kramkarthik.com).</li>
			<li>Port: Leave port as it is unless you have a reason not to.</li>
			<li>User name: User name you use to SSH into your remote machine.</li>
			<li>Password: Corresponding password.</li>
			<li>Private key: This is your id_rsa key value. You need to paste complete contents of it right from '----BEGIN RSA PRIVATE KEY----' till '----END RSA PRIVATE KEY----' including the begin and end tag.</li>
			<p>Click on OK.</p>
		</ul></li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/add_ssh_connection.png" /></p>
	<p><li>Now in the previous tab, you can choose this SSH connection from the dropdown. Enter 'Target folder' value as the folder path to which you deploy static files in the remote machine. In my case, it is '/var/www/kramkarthik.com/html'. Click 'Advanced' and enable the checkbox that you need. I usually want to clean the directory and deploy completely. Also, I want the pipeline to fail if there is no file to copy.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/complete_ssh_configuration.png" /></p>
	<p><li>Change your release pipeline name to something meaningful.</li></p>
	<p><li>We have a build pipeline. We also have a release pipeline. Now we need to configure what triggers a build and what triggers a release.</li></p>
	<p><li>Go to your build pipeline and click on edit.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/build_pipeline_edit.png" /></p>
	<p><li>Click on 'Triggers'. Choose your repository under 'Continuous Integration' and on the right side menu, choose 'Enable continuous integration'. Click on Save. Now any commit or merge to 'master' branch will trigger a build.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/build_trigger.png" /></p>
	<p><li>Now let's edit the 'Release pipeline' and add a trigger. Once you go to edit page of 'Release pipeline', click on the flash sign inside Artifacts box. Switch the toggle to enable 'Continuous deployment trigger'. Click on Save.</li></p>
	<p><img style="height:85%;width:85%" src="/images/deploy_azure_pipelines/deploy_trigger.png" /></p>
</ol>
<p><h4>That's it. You have configured Azure Pipelines to deploy your Hugo static site to your VM successfully.</h4></p>
<p>You can test this by doing a commit to your master branch. It should build and deploy to your remote machine. No more manual generating hugo site and copying files over to the remote machine. Write, commit, push to git and your site should have the changes in less than 2 minutes (based on the size of your site).</p>