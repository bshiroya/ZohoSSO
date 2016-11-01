-------------------------
SAML Configuration Steps
-------------------------
1.Include ZohoSAML.jar inside WebApp Lib
		
2.Add the following filter Mapping inplace of SecurityFilter

	<filter>
		<filter-name>SAMLFILTER</filter-name>
		<filter-class>com.zoho.saml.Samlfilter</filter-class>
		<init-param>
			<param-name>excludePattern</param-name>
			<param-value>\/zoho\/(requestSAML|consumeSAML|logout)</param-value>
		</init-param>
	</filter>
	<filter-mapping>
    	<filter-name>SAMLFILTER</filter-name>
    	<url-pattern>*.do</url-pattern>
  	</filter-mapping>
  	
	<servlet>
		<servlet-name>samlRequest</servlet-name>
		<servlet-class>com.zoho.saml.RequestSAML</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>samlRequest</servlet-name>
		<url-pattern>/zoho/requestSAML</url-pattern>
	</servlet-mapping>

3.Add the following Servlet mapping (SAML Endpoints - Request,Consume,Logout)

	<servlet>
		<servlet-name>samlRequest</servlet-name>
		<servlet-class>com.zoho.saml.RequestSAML</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>samlRequest</servlet-name>
		<url-pattern>/zoho/requestSAML</url-pattern>
	</servlet-mapping>
	
	<servlet>
		<servlet-name>samlResponse</servlet-name>
		<servlet-class>com.zoho.saml.ConsumeSAML</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>samlResponse</servlet-name>
		<url-pattern>/zoho/consumeSAML</url-pattern>
	</servlet-mapping>		

	<servlet>
		<servlet-name>samlLogout</servlet-name>
		<servlet-class>com.zoho.saml.LogoutSAML</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>samlLogout</servlet-name>
		<url-pattern>/zoho/logout</url-pattern>
	</servlet-mapping>		 

4.For All URL's Passing through SamlFilter CurrentUserObject can be obtained using the following Code Snippet

	User user=SamlUtil.getCurrentUser();

-------------------------
OAuth2 Configuration Steps
-------------------------
1.Include ZohoOAuth.jar inside WebApp Lib

	
2.Add the following Servlet mapping (Request OAuth) and configure dispatchTo init param if u need to obtain OAuth tokens in request attribute (Name : OAUTH_PARAMS)

	<servlet>
		<servlet-name>requestOAuth</servlet-name>
		<servlet-class>com.zoho.oauth.OAuth</servlet-class>
        <init-param>
        	<param-name>dispatchTo</param-name>
        	<param-value>/jsp/OAuth.jsp</param-value>
        </init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>requestOAuth</servlet-name>
		<url-pattern>/zoho/requestOAuth</url-pattern>
	</servlet-mapping>

3.implement PersistenceHandler method if u need to persist OAuth data and specify the classpath in Persistence_hanlder Key under property.properties file.


4.set access_type property to offline to generate both Access and refresh token. 