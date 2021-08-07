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
<ul>
	<li>
		<h5>Starting your virtual environment and installing dependencies.</h5>
		<pre>
				<p>Create an isolated Python 3 environment named env with virtualenv:</p>
			<code>
				virtualenv -p python3 env
			</code>

			<p>Enter your newly created virtualenv named env:</p>
			<code>
				source env/bin/activate
			</code>

			<p>Use pip to install dependencies for your project from the requirements.txt file</p>
			<code>
				pip install -r requirements.txt
			</code>
			<p>The requirements.txt file is a list of package dependencies you need for your project. The above command downloaded all of these listed package dependencies to the virtualenv.</p>
		</pre>

	</li>
</ul>

<h3>The requirements.txt file is a list of package dependencies you need for your project. The above command downloaded all of these listed package dependencies to the virtualenv.</h3>
<pre>
	<h5>Next, create an App Engine instance by using:</h5>
	<code>
		gcloud app create
	</code>
	<p>A prompt will display a list of regions. Select a Region that supports App Engine Flexible for Python then press Enter. You can read more about Regions and Zones here.</p>

	<h5>Creating a Storage Bucket</h5>
	<p>First, set the environment variable CLOUD_STORAGE_BUCKET equal to the name of your PROJECT_ID. (It is generally recommended to name your bucket the same as your PROJECT_ID for convenience purposes).</p>
	<code>export CLOUD_STORAGE_BUCKET=${PROJECT_ID}</code>

	<p>Now run the following command to create a bucket with the same name as your PROJECT_ID.</p>
	<code>gsutil mb gs://${PROJECT_ID}</code>

	<h5>Running the Application</h5>
	<p>Execute the following command to start your application:</p>

	<code>python main.py</code>
</pre>
