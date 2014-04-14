OX Install
======
<p>This is a step by step instruction for installing oxserver (oxAuth and oxTrust).</p>
<h2><a name="table-of-contents" class="anchor" href="#table-of-contents"><span class="octicon octicon-link"></span></a>Table of contents</h2>
<ul>
<li><a href="#prerequisites">Prerequisites</a>
  <ul>
    <li><a href="#requirements">Requirements</a></li>
    <li><a href="#setup-opendj">Setup OpenDJ</a></li>
    <li><a href="#setup-maven">Setup Maven 3.x</a></li>
    <li><a href="#setup-tomcat">Setup Tomcat 7</a></li>
  </ul>
</li>
<li><a href="#configure-oxauth">Configure oxAuth</a></li>
<li><a href="#configure-oxtrust">Configure oxTrust</a></li>
<li><a href="#configure-install-script">Configure Install Script</a></li>
<li><a href="#advanced-install-script-configuration">Advanced Install Script Configuration</a>
  <ul>
    <li><a href="#build-project">Build Project</a></li>
    <li><a href="#setup.properties">setup.properties</a></li>
  </ul>
</li>

</ul>
<h2><a name="prerequisites" class="anchor" href="#prerequisites"><span class="octicon octicon-link"></span></a>Prerequisites</h2>
<p>This is a step by step instruction for installing the OX server (oxAuth and oxTrust).</p>
<p>Platform: CentOS 6.4</p>
<h3><a name="quick-start" class="anchor" href="#requirements"><span class="octicon octicon-link"></span></a>Requirements</h3>
<ul>
  <li>OpenJDK 1.6 (Install openJDK1.6 using “yum”) or java7</li>
  <li>OpenDJ 2.7 or 2.6</li>
  <li>Maven 3.x</li>
  <li>Python 2.7.*</li>
  <li>Tomcat 7</li>
</ul>
<p>Make sure your machine name is in hosts file, if not then edit /etc/hosts and add this line at the end of file.</p>
<p>127.0.0.1 <your machine name></p>
<h3><a name="setup-opendj" class="anchor" href="#setup-opendj"><span class="octicon octicon-link"></span></a>Setup OpenDJ</h3>
<ul>
  <li>Download from official openDJ site.</li>
  <li>Extract it.</li>
  <li>Now run this command:</li>
<div class="highlight highlight-bash"><pre><span class="c">$ opendj/setup --cli --baseDN o=gluu --ldapPort 1389 --adminConnectorPort 4444 --rootUserDN cn=Directory\ Manager --rootUserPassword passpass --no-prompt --noPropertiesFile</pre></div>
</ul>
<p>Now you need to stop the server. To do so, run this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ opendj/bin/stop-ds</pre></div>
<p>(note: example password: passpass but you must set your own password)</p>
<h3><a name="setup-maven" class="anchor" href="#setup-maven"><span class="octicon octicon-link"></span></a>Setup Maven 3.x</h3>
<p>If its not already present ($ which mvn) , then check your linux distribution for a package or download it from <a href="http://maven.apache.org/download.cgi">http://maven.apache.org/download.cgi</a></p>
<h3><a name="setup-tomcat" class="anchor" href="#setup-tomcat"><span class="octicon octicon-link"></span></a>Setup Tomcat 7</h3>
<ul>
  <li>Download Tomcat 7</li>
  <li>Extract it.</li>
</ul>
<p>Encrypted Password strings: (download the file <a href="http://ox.gluu.org/lib/exe/fetch.php?media=oxauth:gluu-encryptor.zip">gluu-encryptor.zip</a>)</p>
<p>To get encrypted representation of your password, unzip the file and execute the command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ java -jar Gluu-Encryptor.jar passpass</pre></div>
<p>Example: decrypted password: “passpass” → encrypted password: ukXNiX9stMCTNcwvx4XCaw== </p>
<h2><a name="configure-oxauth" class="anchor" href="#configure-oxauth"><span class="octicon octicon-link"></span></a>Configure oxAuth</h2>
<p>Clone <a href="https://github.com/GluuFederation/oxAuth.git">https://github.com/GluuFederation/oxAuth.git</a> using this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ git clone https://github.com/GluuFederation/oxAuth.git</pre></div>
<p>Edit config-oxauth.properties using this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ vim oxAuth/Server/profiles/setup/config-oxauth.properties</pre></div>
<p>Sample configuration:<p>
<div class="highlight highlight-bash"><pre><span class="c">config.oxauth.issuer=http://localhost:8080
config.oxauth.contextPath=http://localhost:8080
config.oxauth.appliance=@!1111!0002!0085
config.ldap.bindDN=cn=Directory Manager
config.ldap.bindPassword=ukXNiX9stMCTNcwvx4XCaw==
config.ldap.servers=localhost:1389
config.ldap.maxConnections=3
config.ldap.useSSL=false
config.ldap.configurationEntryDN=ou=oxAuth,ou=configuration,o=@!1111,o=gluu
config.ldap.createLdapConfigurationEntryIfNotExist=true
</pre></div>
<h2><a name="configure-oxtrust" class="anchor" href="#configure-oxtrust"><span class="octicon octicon-link"></span></a>Configure oxTrust</h2>
<p>Clone <a href="https://github.com/GluuFederation/oxTrust.git">https://github.com/GluuFederation/oxTrust.git</a> using this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ git clone https://github.com/GluuFederation/oxTrust.git</pre></div>
<p>Edit config-oxtrust.properties using this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ vim oxTrust/profiles/setup/config-oxtrust.properties</pre></div>
<p>Sample configuration:<p>
<div class="highlight highlight-bash"><pre><span class="c">config.ldap.idp.bindPassword=ukXNiX9stMCTNcwvx4XCaw==
config.ldap.idp.servers=localhost\:1389
config.ldap.central.bindPassword=ukXNiX9stMCTNcwvx4XCaw==
config.ldap.central.servers=localhost\:1389
config.appliance.svn_base64_encoded_password=ukXNiX9stMCTNcwvx4XCaw==
config.host.idp_name=localhost:8080
config.host.idp_mysql_base64_encoded_password=ukXNiX9stMCTNcwvx4XCaw==
config.host.idp.ldap_base64_encoded_password=ukXNiX9stMCTNcwvx4XCaw==
config.host.vds.ldap_base64_encoded_password=ukXNiX9stMCTNcwvx4XCaw==
config.host.keystore_password=ukXNiX9stMCTNcwvx4XCaw==
</pre></div>
<h2><a name="configure-install-script" class="anchor" href="#configure-install-script"><span class="octicon octicon-link"></span></a>Configure Install Script</h2>
<p>Clone <a href="https://github.com/GluuFederation/install.git">https://github.com/GluuFederation/install.git</a> using this command:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ git clone https://github.com/GluuFederation/install.git</pre></div>

<p>Edit install/setup.properties</p>
<p>Set these variables:</p>
<ul>
  <li>platform=unix</li>
  <li>dsType=opendj</li>
  <li>dsHome=full path of opendj</li>
  <li>oxAuthHome=full path of oxAuth</li>
  <li>oxTrustHome=full path of oxTrust</li>
  <li>tomcatHome=full path of tomcat</li>
</ul>
<p>Start openDj server:</p> 
<div class="highlight highlight-bash"><pre><span class="c">$ opendj/bin/start-ds
</pre></div>
<p>Run setup:</p>
<div class="highlight highlight-bash"><pre><span class="c">$ python install/setup.py
</pre></div>
<p>Test ox in web browser by loading this URL:<br />
<a href="http://localhost:8080/oxTrust">http://localhost:8080/oxTrust</a></p>
<h2><a name="advanced-install-script-configuration" class="anchor" href="#advanced-install-script-configuration"><span class="octicon octicon-link"></span></a>Advanced Install Script Configuration</h2>
<p>Install script performs a few step which are well described in setup.properties file:</p>
<ol>
  <li>Generates ldap schema</li>
  <li>Generates ldap date</li>
  <li>Configures directory server (in this document we stick to OpenDJ)</li>
  <li>Build OX Products (oxAuth, oxTrust or any other product). This step can be configured (e.g. to build only oxAuth and skip oxTrust.)</li>
  <li>Deploy OX Products to Web Container (Tomcat).</li>
  <li>Start Web Container</li>
</ol>
<p>Please check setup.properties for more details. </p>
<h3><a name="build-project" class="anchor" href="#build-project"><span class="octicon octicon-link"></span></a>Build Project</h3>
<p>Install script builds OX Products with “setup” Maven profile:</p>
<div class="highlight highlight-bash"><pre><span class="c">mvn clean install -Dmaven.test.skip=true -Dcfg=setup
</pre></div>
<h3><a name="setup.properties" class="anchor" href="#setup.properties"><span class="octicon octicon-link"></span></a>setup.properties</h3>
<p>Location: <a href="https://github.com/GluuFederation/install/blob/master/setup.properties">https://github.com/GluuFederation/install/blob/master/setup.properties</a></p>
<p>##############################<br />
###### Control flow <br />
# Script runs sequentially steps:</p>
<ol>
  <li>Generates LDAP schema;</li>
  <li>Generates LDAP data required for correct running of OX products;</li>
  <li>configures Directory Server (with LDAP schema and LDAP data generated in step 1 and 2).</li>
  <li>Builds OX products (e.g. oxAuth, oxTrust)</li>
  <li>Deploy OX products to Application Container(e.g. oxAuth, oxTrust to Tomcat)</li>
  <li>Starts Application Container (Tomcat)</li>
</ol>
<p># ATTENTION: It's possible to switch on/off each step in setup script.<br />
# However you need to be aware that there is dependencies between steps.<br />
##############################</p>
<p># Generates LDAP schema for Directory server. Later it's used for Directory server configuration. <br />
generateSchema=true</p>
<p># Generates LDAP data as LDIF. Later is used to import into Directory server.<br />
generateLdapDataLdif=true</p>
<p># Configures Directory server with LDAP schema and LDAP data generated in previous step.<br />
configureDS=true</p>
<p># Configures Directory server with LDAP schema and LDAP data generated in previous step.<br />
configureDS=true</p>
<p># Deploy OX products.<br />
deployOX=true</p>
<p># Start application container (tomcat).<br />
startContainer=true</p>
<p>##############################<br />
###### Environment and Directory Server configuration<br />
##############################</p>
<p># Platform, possible values: windows, unix.<br />
platform=windows</p>
<p># Directory server name, possible values: opendj, opends, openldap, apacheds.<br />
# ATTENTION : currently ONLY openDJ and openDS are supported<br />
dsType=opendj</p>
<p># Directory server home directory.<br />
dsHome=c:\\own\\java\\opendj-2.4.4\\OpenDJ<br />
ldapHost=localhost<br />
ldapPort=1389<br />
ldapDN=cn=directory manager<br />
ldapPW=pw</p>
<p>##############################<br />
###### Schema generation<br />
##############################<br />
schemaFN=101-ox.ldif<br />
userSchemaFN=100-user.ldif<br />
userSchemaTemplateFN=100-user-template.ldif</p>
<p>##############################<br />
###### LDAP Data generation<br />
##############################<br />
dataTemplateFile=template.ldif<br />
dataGeneratedFile=generated-data.ldif<br />
orgInum=@!1111<br />
orgPass=changeit<br />
orgInumNoDelimiters=1111<br />
suffix=o=gluu<br />
orgName=YOUR ORGANIZATION NAME HERE<br />
orgShortName=yourname<br />
l=NOWHERE<br />
givenName=First<br />
sn=Last<br />
uid=you<br />
mail=you@yoursmtp.any<br />
password=changeit<br />
personInum=@!1111!0000<br />
applianceInum=@!1111!0002<br />
groupInum=@!1111!0003<br />
attributeInum=@!1111!0005<br />
applianceQuad=0085<br />
groupQuad=20A0<br />
personQuad=C975</p>
<p># manager group inum -> used to assign user to manager group, without it oxTrust will not represent complete UI for configuration<br />
managerGroupInum=@!1111!0003!B2C6</p>
<p># ATTENTION : Client is restricted to localhost ONLY<br />
oxTrustClientId=@!1111!0008!1234!1234</p>
<p># ATTENTION : Client is restricted to localhost ONLY<br />
# Encoded 12345678-1234-1234-1234-123456789012<br />
oxTrustClientSecret=HdUJNbcCCEuZVGC3SjE6imo5fzDeQTV5HdUJNbcCCEs8n8r/51LyJA==</p>
<p>##############################<br />
###### Build process<br />
# To successfully build OX make sure your Maven is properly configured.<br />
# If you don't have Maven installed, please install it and configure<br />
# as described here: http://maven.apache.org/download.cgi<br />
##############################</p>
<p># Path to oxAuth directory with sources.<br />
# If you don't have sources please download them from here: <a href="https://svn.gluu.info/repository/openxdi/oxAuth/">https://svn.gluu.info/repository/openxdi/oxAuth/</a><br />
oxAuthHome=u:\\own\\project\\oxAuth</p>
<p># Path to oxTrust directory with sources.<br />
# If you don't have sources please download them from here: <a href="https://svn.gluu.info/repository/openxdi/oxTrust/">https://svn.gluu.info/repository/openxdi/oxTrust/</a><br />
oxTrustHome=u:\\own\\project\\all\\oxTrust</p>
<p>##############################<br />
###### Deploy process<br />
# To successfully deploy OX products make sure your Tomcat is properly configured.<br />
# If you don't have Tomcat installed, please install and configure it as described here: <a href="http://tomcat.apache.org/download-60.cgi">http://tomcat.apache.org/download-60.cgi</a><br />
##############################</p>
<p># Tomcat home directory<br />
tomcatHome=u:\\own\\java\\apache-tomcat-6.0.33_setup</p>
<p># Java runtime options used when the "start", "stop", or "run" command is executed of Tomcat catalina.<br />
tomcatJavaOpts=-Xms228M -Xmx1512M -XX:MaxPermSize=292M</p>











