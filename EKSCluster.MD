<p><span style="font-size:16px"><span style="font-family:Arial"><span style="color:#000000">To setup a simple EKS cluster with nginx 3 replica sets and accessible only through a load balancer.</span></span></span></p>

<h3><span style="font-size:16px"><span style="font-family:Arial"><span style="color:#434343">Create an IAM User with Admin Permissions and&nbsp;</span><span style="color:#000000">Copy the access key and secret ID</span></span></span></h3>

<p>&nbsp;</p>

<h3><span style="font-size:13.999999999999998pt"><span style="font-family:Arial"><span style="color:#434343">Launch an EC2 Instance and Configure the Command Line Tools</span></span></span></h3>

<ol>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Navigate to EC2 &gt; Instances.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">In the command line window, check the AWS CLI version:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">aws --version</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">It should be an older version.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Download v2:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">curl &quot;https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip&quot; -o &quot;awscliv2.zip&quot;</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">unzip awscliv2.zip</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">See where the current AWS CLI is installed:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">which aws</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">It should be /usr/bin/aws.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Update it:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">sudo ./aws/install --bin-dir /usr/bin --install-dir /usr/bin/aws-cli --update</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Check the version of AWS CLI:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">aws --version</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">It should now be updated.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Configure the CLI:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">aws configure</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">For AWS Access Key ID, paste in the access key ID you copied earlier.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">For AWS Secret Access Key, paste in the secret access key you copied earlier.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">For Default region name, enter us-east-1.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">For Default output format, enter json.</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Download kubectl:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.16.8/2020-04-16/bin/linux/amd64/kubectl</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Apply execute permissions to the binary:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">chmod +x ./kubectl</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Copy the binary to a directory in your path:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">mkdir -p $HOME/bin &amp;&amp; cp ./kubectl $HOME/bin/kubectl &amp;&amp; export PATH=$PATH:$HOME/bin</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Ensure kubectl is installed:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">kubectl version --short --client</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Download eksctl:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">curl --silent --location &quot;https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz&quot; | tar xz -C /tmp</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Move the extracted binary to /usr/bin:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">sudo mv /tmp/eksctl /usr/bin</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Get the version of eksctl:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">eksctl version</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">See the options with eksctl:</span></span></span></li>
	<li style="list-style-type:decimal"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">eksctl help</span></span></span></li>
</ol>

<h3>&nbsp;</h3>

<h3><strong><span style="font-size:13.999999999999998pt"><span style="font-family:Arial"><span style="color:#434343">Provision an EKS Cluster</span></span></span></strong></h3>

<ul>
	<li><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Use the below command to provision an EKS cluster with three worker nodes in us-east-1 from the ec2 instance we created earlier for admin control.</span></span></span></li>
</ul>

<blockquote>
<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">eksctl create cluster --name dev --version 1.21 --region us-east-1 --nodegroup-name standard-workers --node-type t3.micro --nodes 3 --nodes-min 1 --nodes-max 4 --managed</span></span></span></p>
</blockquote>

<p>&nbsp;</p>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">It can take around 15 mins for the setup to get completed.In the AWS Management Console, navigate to CloudFormation and take a look at what&rsquo;s going on there.Select the eksctl-dev-cluster stack (this is our control plane). Click Events, so you can see all the resources that are being created. Once both stacks are complete, navigate to Elastic Kubernetes Service &gt; Clusters. Click the listed cluster. Navigate through various tabs and check the resources which are automatically created for our cluster. Navigate to EC2 &gt; Instances, where you should see the instances have been launched.&nbsp;</span></span></span></p>

<p>&nbsp;</p>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">In the CLI, check the cluster:</span></span></span></p>

<blockquote>
<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">eksctl get cluster</span></span></span></p>
</blockquote>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Enable it to connect to our cluster:</span></span></span></p>

<blockquote>
<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">aws eks update-kubeconfig --name dev --region us-east-1</span></span></span></p>
</blockquote>

<h3>&nbsp;</h3>

<h3><span style="font-size:13.999999999999998pt"><span style="font-family:Arial"><span style="color:#434343">Create a Deployment on Your EKS Cluster</span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Take a look at the deployment file:</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">cat nginx-deployment.yaml</span></span></span></p>

<blockquote>
<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    env: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      env: dev
  template:
    metadata:
      labels:
        env: dev
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80</pre>
</blockquote>

<p>&nbsp;</p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Take a look at the service file:</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">cat nginx-svc.yaml</span></span></span></p>

<blockquote>
<pre>
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  labels:
    env: dev
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    env: dev</pre>
</blockquote>

<p>&nbsp;</p>

<p>&nbsp;</p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Create the service first&nbsp;</span></span></span></p>

<blockquote>
<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">kubectl apply -f ./nginx-svc.yaml</span></span></span></p>
</blockquote>

<p>&nbsp;</p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">And then</span></span></span></p>

<blockquote>
<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">kubectl apply -f ./nginx-deployment.yaml</span></span></span></p>
</blockquote>

<p><br />
<span style="color:#000000; font-family:Arial"><span style="font-size:14.6667px">To list the nodes and pods created:</span></span></p>

<blockquote>
<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">kubectl get node</span></span></span></p>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Kubectl get pods</span></span></span></p>
</blockquote>

<p>&nbsp;</p>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Access the application using the load balancer, replacing &lt;LOAD_BALANCER_DNS_HOSTNAME&gt; with the IP you copied earlier:</span></span></span></p>

<p style="margin-left:48px"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">curl &quot;&lt;LOAD_BALANCER_DNS_HOSTNAME&gt;&quot;</span></span></span></p>

<p><br />
&nbsp;</p>
