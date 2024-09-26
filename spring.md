## Spring Security Overview

### UserDetailsService
UserDetailsService is an interface that I implement to provide a way for Spring Security to retrieve user-specific data. By defining the loadUserByUsername(String username) method, I enable the authentication process to fetch user details like username, password, and authorities from my data source, such as a database. 

### SecurityContextHolder
SecurityContextHolder is a core class in Spring Security that stores the security context of the current execution thread, including the details of the authenticated user. When I need to access information about the currently logged-in user, I can retrieve it from the SecurityContextHolder. This allows me to obtain the Authentication object, which contains the user's credentials and granted authorities, enabling me to make authorization decisions.
