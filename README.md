# deploying-a-python-flask-web-application-to-app-engine

<p>If you want to deploy your python flask web application on GCP App Engine, then follow this article.</p>
<p> To deploy flask application on GCP App Engine will be require GCP account.</p>
<h3>What is App Engine</h3>
<p>Google App Engine applications are easy to create, easy to maintain, and easy to scale as your traffic and data storage needs change. With App Engine, there are no servers to maintain. You simply upload your application and it's ready to go.

App Engine applications automatically scale based on incoming traffic. Load balancing, microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, search, versioning, roll out and roll backs, and security scanning are all supported natively and are highly customizable.

App Engine's Flexible Environment supports a host of programming languages, including Java, Python, PHP, NodeJS, Ruby, and Go. App Engine's Standard Environment is an additional option for certain languages including Python. The two environments give users maximum flexibility in how their application behaves since each environment has certain strengths. Read Choosing an App Engine Environment for more information.
</p>
<h3>Authenticate API Requests</h3>
<p>Enable required API, To enable API goto <b>Navigation Button-> API & Services -> Library</b> then search API and click on enable butoon.</p>

<ul> Steps for deploying flask web apploication on GCP
<li>Set an environment variable for [YOUR_PROJECT_ID], replacing [YOUR_PROJECT_ID] with your own project ID:</li>
<pre>
	<code>
	export PROJECT_ID=[YOUR_PROJECT_ID]
</code>
</pre>
<li>Create a Service Account to access the Google Cloud APIs when testing locally:
	<pre>
		<code>
			gcloud iam service-accounts create qwiklab \
  			--display-name "My Qwiklab Service Account"
		</code>
	</pre>
	Give your newly created Service Account appropriate permissions:
	<pre>
		<code>
			gcloud projects add-iam-policy-binding ${PROJECT_ID} \
			--member serviceAccount:qwiklab@${PROJECT_ID}.iam.gserviceaccount.com \
			--role roles/owner
		</code>
	</pre>
	After creating your Service Account, create a Service Account key:
	<pre>
		<code>
			gcloud iam service-accounts keys create ~/key.json \
			--iam-account qwiklab@${PROJECT_ID}.iam.gserviceaccount.com
		</code>
	</pre>
<p>Above command generates a service account key stored in a JSON file named key.json in your home directory.

Using the absolute path of the generated key, set an environment variable for your service account key:
<pre>
	<code>
		export GOOGLE_APPLICATION_CREDENTIALS="/home/${USER}/key.json"
	</code>
</pre>

</p>
<li>
</ul>
<h3>Testing the Application Locally</h3>

<h5>Starting your virtual environment and installing dependencies.</h5>

<p>Create an isolated Python 3 environment named env with virtualenv:</p>
<pre>
	<code>
		virtualenv -p python3 env
	</code>
</pre>

<p>Enter your newly created virtualenv named env:</p>
<pre>
	<code>
		source env/bin/activate
	</code>
</pre>
<p>Use pip to install dependencies for your project from the requirements.txt file</p>
<pre>
	<code>
		pip install -r requirements.txt
	</code>
</pre>
<p>The requirements.txt file is a list of package dependencies you need for your project. The above command downloaded all of these listed package 				dependencies to the virtualenv.</p>
<p>The requirements.txt file is a list of package dependencies you need for your project. The above command downloaded all of these listed package dependencies to the 		virtualenv.</p>

<h5>Next, create an App Engine instance by using:</h5>
	<pre>
	<code>
		gcloud app create
	</code>
	</pre>
	<p>A prompt will display a list of regions. Select a Region that supports App Engine Flexible for Python then press Enter. You can read more about Regions and Zones 		here.</p>

<h5>Creating a Storage Bucket</h5>
	<p>First, set the environment variable CLOUD_STORAGE_BUCKET equal to the name of your PROJECT_ID. (It is generally recommended to name your bucket the same as your 		PROJECT_ID for convenience purposes).</p>
	<pre>
	<code>export CLOUD_STORAGE_BUCKET=${PROJECT_ID}</code>
	</pre>
	<p>Now run the following command to create a bucket with the same name as your PROJECT_ID.</p>
	<pre>
	<code>gsutil mb gs://${PROJECT_ID}</code>
	</pre>
	<h5>Running the Application</h5>
	<p>Execute the following command to start your application:</p>
	<pre>
	<code>python main.py</code>
	</pre>
	<p>Once the application starts, click on the Web Preview icon in the Cloud Shell toolbar and choose "Preview on port 8080."</p>
	<p>A tab in your browser opens and connects to the server you just started. You should see something like your app , after you van test the your app.</p>


<h5>Exploring the Code</h5>
	
<p>Sample Code Layout
The sample has the following layout:</p>
<pre>
<code>
	templates/
	homepage.html   /* HTML template that uses Jinja2 */
	app.yaml          /* App Engine application configuration file */
	main.py           /* Python Flask web application */
	requirements.txt  /* List of dependencies for the project */
</code>
</pre>

<h5>Deploying the App to App Engine Flexible</h5>
	
<p>App Engine Flexible uses a file called app.yaml to describe an application's deployment configuration. If this file is not present, App Engine will try to guess the 	deployment configuration. However, it is a good idea to provide this file.

Next, you will modify app.yaml using an editor of your choice vim, nano, or emacs. We will use the nano editor:</p>
<pre>
<code>nano app.yaml</code>
</pre>
<p>Once you have app.yaml open, replace <your-cloud-storage-bucket> with the name of your Cloud Storage bucket. (If you forgot the name of your Cloud Storage bucket, 		copy the Project ID from the Qwiklabs tab). The env_variables section sets up environment variables that will be used in main.py once the application is deployed.</p>
<pre>
<code>
	runtime: python
	env: flex
	entrypoint: gunicorn -b :$PORT main:app

	runtime_config:
	    python_version: 3

	env_variables:
CLOUD_STORAGE_BUCKET: <your-cloud-storage-bucket>
</code>
</pre>

<p>This is the basic configuration needed to deploy a Python 3 App Engine Flex application. You can learn more about configuring App Engine here.

You can now save and close the file in nano by using (Ctrl + x), which will prompt:
Type a letter Y and then press the ENTER key one more time to confirm the filename for the following prompt:
</p>

<p>Update your Cloud Build timeout:</p>
<pre>
<code>gcloud config set app/cloud_build_timeout 1000</code>
</prfe>
<p>Deploy your app on App Engine by using gcloud:</p>
<pre>
<code>gcloud app deploy</code>
</pre>
<p>If asked, Do you want to continue (Y/n), press Y and then Enter.

Watch in Cloud Shell as the application gets built. This will take up to 10 minutes. The App Engine Flexible environment is automatically provisioning a Compute Engine 	virtual machine for you behind the scenes, and then installing the application, then starting it.</p>
<p>After the application is deployed, open the app in your web browser with the following URL:</p>
<pre>
<code>
	https://<PROJECT_ID>.appspot.com
</code>
</pre>
<p>Thank You</p>

