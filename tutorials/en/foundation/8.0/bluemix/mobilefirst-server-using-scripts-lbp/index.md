---
layout: tutorial
title: Setting Up MobileFirst Server on Bluemix using Scripts for Liberty for Java
breadcrumb_title: Liberty for Java
relevantTo: [ios,android,windows,javascript]
weight: 3
---
<!-- NLS_CHARSET=UTF-8 -->
## Overview
{: #overview }
Follow the instructions below to configure a {{ site.data.keys.mf_server }} instance on a Liberty for Java runtime on Bluemix. ({{ site.data.keys.mf_analytics }} instances can be run on IBM containers only.) To achieve this you will go through the following steps:

* Setup your host computer with the required tools (Cloud Foundry CLI)
* Setup your Bluemix account
* Build a {{ site.data.keys.mf_server }} and push it to the Bluemix as a Cloud Foundry application.

Finally, you will register your mobile apps as well as deploy your adapters.

**Notes:**  

* Windows OS is currently not supported for running these scripts.  
* The {{ site.data.keys.mf_server }} Configuration tools cannot be used for deployments to Bluemix.

#### Jump to:
{: #jump-to }

* [Register an account at Bluemix](#register-an-account-at-bluemix)
* [Set up your host machine](#set-up-your-host-machine)
* [Download the {{ site.data.keys.mf_bm_pkg_name }} archive](#download-the-ibm-mfpf-container-8000-archive)
* [Adding Analytics server information](#adding-analytics-server-configuration-to-mobilefirst-server)
* [Applying {{ site.data.keys.mf_server }} Fixes](#applying-mobilefirst-server-fixes)
* [Removing the database service configuration from Bluemix](#removing-the-database-service-configuration-from-bluemix)

## Register an account at Bluemix
{: #register-an-account-at-bluemix }
If you do not have an account yet, visit the [Bluemix website](https://bluemix.net) and click **Get Started Free** or **Sign Up**. You need to fill up a registration form before you can move on to the next step.

### The Bluemix Dashboard
{: #the-bluemix-dashboard }
After signing in to Bluemix, you are presented with the Bluemix Dashboard, which provides an overview of the active Bluemix **space**. By default, this work area receives the name "dev". You can create multiple work areas/spaces if needed.

## Set up your host machine
{: #set-up-your-host-machine }
To manage the Bluemix Cloud Foundry app, you need to install the Cloud Foundry CLI.  
You can run the scripts using the macOS Terminal.app or a Linux bash shell.

Install the [Cloud Foundry CLI](https://github.com/cloudfoundry/cli/releases?cm_mc_uid=85906649576514533887001&cm_mc_sid_50200000=1454307195).

## Download the {{ site.data.keys.mf_bm_pkg_name }} archive
{: #download-the-ibm-mfpf-container-8000-archive}
To set up {{ site.data.keys.product }} on Liberty on Java, you must first create a file layout that will later be pushed to Bluemix.  
<a href="http://www-01.ibm.com/support/docview.wss?uid=swg2C7000005" target="blank">Follow the instructions in this page</a> to download the {{ site.data.keys.mf_server }} 8.0 for IBM Containers archive (.zip file, search for: *CNBL0EN*).

The archive file contains the files for building an file layout (**dependencies** and **mfpf-libs**), the files for building and deploying a {{ site.data.keys.mf_analytics }} Container (**mfpf-analytics**) and files for configuring a {{ site.data.keys.mf_server }} Cloud Foundry app (**mfpf-server-libertyapp**).

<div class="panel-group accordion" id="terminology" role="tablist" aria-multiselectable="false">
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="zip-file">
            <h4 class="panel-title">
                <a class="preventScroll" role="button" data-toggle="collapse" data-parent="#zip-file" data-target="#collapse-zip-file" aria-expanded="false" aria-controls="collapse-adapter-xml"><b>Click to read more about the archive file contents</b></a>
            </h4>
        </div>

        <div id="collapse-zip-file" class="panel-collapse collapse" role="tabpanel" aria-labelledby="zip-file">
            <div class="panel-body">
                <img src="zip.png" alt="Image showing the file system structure of the archive file" style="float:right;width:570px"/>
                <h4>dependencies folder</h4>
                <p>Contains the {{ site.data.keys.product }} runtime and IBM Java JRE 8.</p>

                <h4>mfpf-libs folder</h4>
                <p>Contains {{ site.data.keys.product_adj }} product component libraries and CLI.</p>

                <h4>mfpf-server-libertyapp folder</h4>

                <ul>

                    <li><b>scripts</b> folder: This folder contains the <b>args</b> folder, which contains a set of configuration files. It also contains scripts to run for logging into Bluemix, building a {{ site.data.keys.product }} app for pushing to BLuemix and running the server on Bluemix. You can choose to run the scripts interactively or by preconfiguring the configuration files as is further explained later. Other than the customizable args/*.properties files, do not modify any elements in this folder. For script usage help, use the <code>-h</code> or <code>--help</code> command-line arguments (for example, <code>scriptname.sh --help</code>).</li>
                    <li><b>usr</b> folder:
                        <ul>
                            <li><b>config</b> folder: Contains the server configuration fragments (keystore, server properties, user registry) used by {{ site.data.keys.mf_server }}.</li>
                            <li><b>keystore.xml</b> - the configuration of the repository of security certificates used for SSL encryption. The files listed must be referenced in the ./usr/security folder.</li>
                            <li><b>mfpfproperties.xml</b> - configuration properties for {{ site.data.keys.mf_server }}. See the supported properties listed in these documentation topics:
                                <ul>
                                <li><a href="../../installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-server-administration-service">List of JNDI properties for {{ site.data.keys.mf_server }} administration service</a></li>
                                    <li><a href="../../installation-configuration/production/server-configuration/#list-of-jndi-properties-for-mobilefirst-runtime">List of JNDI properties for the {{ site.data.keys.product_adj }} runtime</a></li>
                                </ul>
                            </li>
                            <li><b>registry.xml</b> - user registry configuration. The basicRegistry (a basic XML-based user-registry configuration is provided as the default. User names and passwords can be configured for basicRegistry or you can configure ldapRegistry.</li>
                        </ul>
                    </li>
                    <li><b>env</b> folder: Contains the environment properties used for server initialization (server.env) and custom JVM options (jvm.options).
                    <br/>
                    </li>

                    <li><b>security</b> folder: used to store the key store, trust store, and the LTPA keys files (ltpa.keys).</li>

                </ul>
				<br/>
                <a class="preventScroll" role="button" data-toggle="collapse" data-parent="#zip-file" data-target="#collapse-zip-file" aria-expanded="false" aria-controls="collapse-zip-file"><b>Close section</b></a>
            </div>
        </div>
    </div>
</div>


## Setting Up the {{ site.data.keys.mf_server }}
{: #setting-up-the-mobilefirst-server }
You can choose to run the scripts interactively or by using the configuration files:
A good place to start is to run the scripts interactively once, which will also record the arguments (**recorded-args**). You can later use the args files to run the scripts in a non interactive mode.

> **Note:** Passwords are not recorded and you will have to manually add the passwords to the argument files.

* Using the configuration files - run the scripts and pass the respective configuration file as an argument.
* Interactively - run the scripts without any arguments.

If you choose to run the scripts interactively, you can skip the configuration but it is strongly suggested to at least read and understand the arguments that you will need to provide.

### {{ site.data.keys.mf_server }}
{: #mobilefirst-server }
<div class="panel-group accordion" id="scripts2" role="tablist" aria-multiselectable="false">
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="step-foundation-1">
            <h4 class="panel-title">
                <a class="preventScroll" role="button" data-toggle="collapse" data-parent="#scripts2" data-target="#collapse-step-foundation-1" aria-expanded="false" aria-controls="collapse-step-foundation-1">Using the configuration files</a>
            </h4>
        </div>

        <div id="collapse-step-foundation-1" class="panel-collapse collapse" role="tabpanel" aria-labelledby="setupCordova">
            <div class="panel-body">
            The <b>args</b> folder contains a set of configuration files which contain the arguments that are required to run the scripts. You can find the empty template files and the explanation of the arguments in the <b>args</b> folder, or after running the scripts interactively in the <b>recorded-args</b> folder. Following are the files:<br/>

              <h4>initenv.properties</h4>
              This file contains properties used to run the environment initialization.
              <h4>prepareserverdbs.properties</h4>
              The {{ site.data.keys.mf_bm_short }} service requires an external <a href="https://console.ng.bluemix.net/catalog/services/dashdb/" target="\_blank">dashDB Enterprise Transactional database instance</a> (Any plan that is marked OLTP or Transactional).<br/>
              <b>Note:</b> The deployment of the dashDB Enterprise Transactional plans is immediate for the plans marked "pay as you go". Make sure you pick one of the suitable plans like <i>Enterprise for Transactions High Availability 2.8.500 (Pay per use)</i> <br/><br/>
              After you have set up your dashDB instance, provide the required arguments.

              <h4>prepareserver.properties</h4>
              This file is used for the prepareserver.sh script. This prepares the server file layout and pushes it to Bluemix as a Cloud Foundry app.
              <h4>startserver.properties</h4>
              This file configures tha runtime attributes of the server and starts is. It is strongly recomended that you use a minimum of 1024 MB (<b>SERVER_MEM=1024</b>) and 3 nodes for high availablilty (<b>INSTANCES=3</b>)

            </div>
        </div>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="step-foundation-2">
            <h4 class="panel-title">
                <a class="preventScroll" role="button" data-toggle="collapse" data-parent="#scripts2" data-target="#collapse-step-foundation-2" aria-expanded="false" aria-controls="collapse-step-foundation-2">Running the scripts</a>
            </h4>
        </div>

        <div id="collapse-step-foundation-2" class="panel-collapse collapse" role="tabpanel" aria-labelledby="setupCordova">
            <div class="panel-body">
              <p>The following instructions demonstrate how to run the scripts by using the configuration files. A list of command-line arguments is also available should you choose to run without in interactive mode:</p>
              <ol>
                  <li><b>initenv.sh – Logging in to Bluemix </b><br />
                      Run the <b>initenv.sh</b> script to login to Bluemix. Run this for the Org and space where your dashDB service is bound:
{% highlight bash %}
./initenv.sh args/initenv.properties
{% endhighlight %}

                        You an also pass the parameters on the commandline

{% highlight bash %}
initenv.sh --user Bluemix_user_ID --password Bluemix_password --org Bluemix_organization_name --space Bluemix_space_name
{% endhighlight %}

                        To learn all the parameters supported and their documentation run the help option

{% highlight bash %}
./initenv.sh --help
{% endhighlight %}
                  </li>
                  <li><b>prepareserverdbs.sh - Prepare the {{ site.data.keys.mf_server }} database</b><br />
                  The <b>prepareserverdbs.sh</b> script is used to configure your {{ site.data.keys.mf_server }} with the dashDB database service or a accessible DB2 database server. The DB2 option is usable particularly when you are running Bluemix local in the same datacentre where you have the DB2 server installed. If using the dashDB service, the service instance of the dashDB service should be available in the Organization and Space that you logged in to in step 1. Run the following:
{% highlight bash %}
./prepareserverdbs.sh args/prepareserverdbs.properties
{% endhighlight %}

                        You an also pass the parameters on the commandline

{% highlight bash %}
prepareserverdbs.sh --admindb MFPDashDBService
{% endhighlight %}

                        To learn all the parameters supported and their documentation run the help option

{% highlight bash %}
./prepareserverdbs.sh --help
{% endhighlight %}

                  </li>
                  <li><b>initenv.sh(Optional) – Logging in to Bluemix</b><br />
                      This step is required only if you need to create your server in a different Organization and Space than where the dashDB service instance is available. If yes, then update the initenv.properties with the new Organization and Space where the containers have to be created (and started), and rerun the <b>initenv.sh</b> script:
{% highlight bash %}
./initenv.sh args/initenv.properties
{% endhighlight %}
                  </li>
                  <li><b>prepareserver.sh - Prepare a {{ site.data.keys.mf_server }}</b><br />
                    Run the <b>prepareserver.sh</b> script in order to build a {{ site.data.keys.mf_server }} and push it to  Bluemix as a Cloud Foundry application. To view all the Cloud Foundry applications and theur URLs in the logged in Org and space, run: <code>cf apps</code><br/>


{% highlight bash %}
./prepareserver.sh args/prepareserver.properties
{% endhighlight %}

                        You an also pass the parameters on the commandline

{% highlight bash %}
prepareserver.sh --name APP_NAME
{% endhighlight %}

                        To learn all the parameters supported and their documentation run the help option

{% highlight bash %}
./prepareserver.sh --help
{% endhighlight %}                  

                  </li>
                  <li><b>startserver.sh - Starting the server</b><br />
                  The <b>startserver.sh</b> script is used to start the {{ site.data.keys.mf_server }} on Liberty for Java Cloud Foundry application. Run:<p/>
{% highlight bash %}
./startserver.sh args/startserver.properties
{% endhighlight %}

                        You an also pass the parameters on the commandline

{% highlight bash %}
./startserver.sh --name APP_NAME
{% endhighlight %}

                        To learn all the parameters supported and their documentation run the help option

{% highlight bash %}
./startserver.sh --help
{% endhighlight %}   

                  </li>
              </ol>
            </div>
        </div>
    </div>
</div>


Launch the {{ site.data.keys.mf_console }} by loading the following URL: `http://APP_HOST.mybluemix.net/mfpconsole` (it may take a few moments).  
Add the remote server by following the instructions in the [Using {{ site.data.keys.mf_cli }} to Manage {{ site.data.keys.product_adj }} Artifacts](../../application-development/using-mobilefirst-cli-to-manage-mobilefirst-artifacts/#add-a-new-server-instance) tutorial.  

With {{ site.data.keys.mf_server }} running on IBM Bluemix, you can now start your application development.

#### Applying changes
{: #applying-changes }
You may need to apply changes to the server layout after you have deployed the server once, e.g: you want to update the analytics URL in **/usr/config/mfpfproperties.xml**. Make the changes and then re-run the following scripts with the same set of arguments.

1. ./prepareserver.sh
2. ./startserver.sh

### Adding analytics server configuration to {{ site.data.keys.mf_server }}
{: #adding-analytics-server-configuration-to-mobilefirst-server }
If you have setup a Analytics server and want to connect it to this {{ site.data.keys.mf_server }} then edit the fie **mfpfproperties.xml** in the folder **package_root/mfpf-server-libertyapp/usr/config** as specified below. Replace the tokens marked with `<>` with correct values from yur deployment.

```xml
<jndiEntry jndiName="${env.MFPF_RUNTIME_ROOT}/mfp.analytics.url" value='"https://<AnalyticsContainerGroupRoute>:443/analytics-service/rest"'/>
<jndiEntry jndiName="${env.MFPF_RUNTIME_ROOT}/mfp.analytics.console.url" value='"https://<AnalyticsContainerPublicRoute>:443/analytics/console"'/>
<jndiEntry jndiName="${env.MFPF_RUNTIME_ROOT}/mfp.analytics.username" value='"<AnalyticsUserName>"'/>
<jndiEntry jndiName="${env.MFPF_RUNTIME_ROOT}/mfp.analytics.password" value='"<AnalyticsPassword>"'/>


<jndiEntry jndiName="${env.MFPF_PUSH_ROOT}/mfp.push.analytics.endpoint" value='"https://<AnalyticsContainerGroupRoute>:443/analytics-service/rest"'/>
<jndiEntry jndiName="${env.MFPF_PUSH_ROOT}/mfp.push.services.ext.analytics" value="com.ibm.mfp.push.server.analytics.plugin.AnalyticsPlugin"/>
<jndiEntry jndiName="${env.MFPF_PUSH_ROOT}/mfp.push.analytics.user" value='"<AnalyticsUserName>"'/>
<jndiEntry jndiName="${env.MFPF_PUSH_ROOT}/mfp.push.analytics.password" value='"<AnalyticsPassword>"'/>
```

## Applying {{ site.data.keys.mf_server }} Fixes
{: #applying-mobilefirst-server-fixes }
Interim fixes for the {{ site.data.keys.mf_server }} on Bluemix can be obtained from [IBM Fix Central](http://www.ibm.com/support/fixcentral).  
Before you apply an interim fix, back up your existing configuration files. The configuration files are located in the
**package_root/mfpf-server-libertyapp/usr** folders.

1. Download the interim fix archive and extract the contents to your existing installation folder, overwriting the existing files.
2. Restore your backed-up configuration files into the  **/mfpf-server-libertyapp/usr** folders, overwriting the newly installed configuration files.

You can now build and deploy the updatd server.

## Removing the database service configuration from Bluemix
{: #removing-the-database-service-configuration-from-bluemix }
If you ran the **prepareserverdbs.sh** script during the configuration of the {{ site.data.keys.mf_server }} image, the configurations and database tables required for {{ site.data.keys.mf_server }} are created. This script also creates the database schema for the {{ site.data.keys.mf_server }}.

To remove the database service configuration from Bluemix, perform the following procedure using Bluemix dashboard.

1. From the Bluemix dashboard, select the dashDB service you have used. Choose the dashDB service name that you had provided as parameter while running the **prepareserverdbs.sh** script.
2. Launch the dashDB console to work with the schemas and database objects of the selected dashDB service instance.
3. Select the schemas related to IBM {{ site.data.keys.mf_server }} configuration. The schema names are ones that you have provided as parameters while running the **prepareserverdbs.sh** script.
4. Delete each of the schema after carefully inspecting the schema names and the objects under them. The database configurations are removed from Bluemix.
