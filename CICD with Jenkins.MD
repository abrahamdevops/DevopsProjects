<h1>CICD with Jenkins, GitHub to a Tomcat server on AWS</h1>

<p>We know how to use work with each and Git, Jenkins independently. What if you want to collaborate these two. You can automate the source code fetching from Github repo using jenkins job, which will then push the built code to a tomcat server. Follow below steps if you are a new guy to DevOps. You will find it easy to follow</p>

<h4>Prerequisites</h4>

<ol>
	<li>EC2 instance with tomcat installation&nbsp;</li>
	<li>Jenkins server</li>
</ol>

<h3>Part-01 : Adding steps for Integration</h3>

<h3>Steps to create Jenkin job</h3>

<ol>
	<li>Login to Jenkins console</li>
	<li>Create&nbsp;<em>Jenkins job</em>, Fill the following details,
	<ul>
		<li><em>Source Code Management:</em>
		<ul>
			<li>Repository:&nbsp;<a href="https://github.com/abrahamdevops/repoVlaxy.git">https://github.com/abrahamdevops/repoVlaxy.git</a></li>
			<li>Branches to build :&nbsp;<code>*/master</code></li>
		</ul>
		</li>
		<li><em>Build:</em>
		<ul>
			<li>Root POM:<code>pom.xml</code></li>
			<li>Goals and options :&nbsp;<code>clean install package</code></li>
		</ul>
		</li>
	</ul>
	</li>
</ol>

<h3>Part-02: Adding Deployment Steps</h3>

<p>in this port we are going to install &#39;deploy to container&#39; plugin. this is need to deploy on tomcat server which we are using.</p>

<ul>
	<li>Install maven plugin without restart
	<ul>
		<li><code>Manage Jenkins</code>&nbsp;&gt;&nbsp;<code>Jenkins Plugins</code>&nbsp;&gt;&nbsp;<code>available</code>&nbsp;&gt;&nbsp;<code>deploy to container</code></li>
	</ul>
	</li>
</ul>

<p>To deploy our build artifacts on tomcat server our Jenkins server need access. For this we should setup credentials. This option is available in Jenkins home page</p>

<ul>
	<li>setup credentials
	<ul>
		<li><code>credentials</code>&nbsp;&gt;&nbsp;<code>jenkins</code>&nbsp;&gt;&nbsp;<code>Global credentials</code>&nbsp;&gt;&nbsp;<code>add credentials</code>

		<ul>
			<li>Username :&nbsp;<code>deployer</code></li>
			<li>Password :&nbsp;<code>XXXXXXX</code></li>
			<li>id :&nbsp;<code>Tomcat_user</code></li>
			<li>Description:&nbsp;<code>Tomcat user to deploy on tomcat server</code></li>
		</ul>
		</li>
	</ul>
	</li>
</ul>

<p>Modify the same job which created in part-01 and add deployment steps.</p>

<ul>
	<li>Post Steps
	<ul>
		<li>Deploy war/ear to container
		<ul>
			<li>WAR/EAR files :&nbsp;<code>**/*.war</code></li>
			<li>Containers :&nbsp;<code>Tomcat 8.x</code>
			<ul>
				<li>Credentials:&nbsp;<code>Tomcat_user</code>&nbsp;(which created in above step)</li>
				<li>Tomcat URL :&nbsp;<code>http://&lt;PUBLIC_IP&gt;:&lt;PORT_NO&gt;</code></li>
			</ul>
			</li>
		</ul>
		</li>
	</ul>
	</li>
</ul>

<p>Save and run the job now.</p>

<h3>Port-03 : Continuous Integration &amp; Continuous Deployment (CI/CD)</h3>

<p>Now job is running fine but to make this as Continuous Integration and Continuous Deployment Tod do that go back and modify job as below.</p>

<ul>
	<li>Build Triggers
	<ul>
		<li>Poll SCM
		<ul>
			<li>schedule&nbsp;<code>*/2 * * * *</code></li>
		</ul>
		</li>
	</ul>
	</li>
</ul>

<p>Save the job and modify the code in GitHub. Then you could see your job get trigger a build without any manual intervention.</p>
