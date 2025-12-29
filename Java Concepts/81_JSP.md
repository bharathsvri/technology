# JSP (JavaServer Pages)

## What is JSP?
JSP allows embedding Java code in HTML pages for dynamic web content.

## Basic JSP
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>Hello JSP</title>
</head>
<body>
    <h1>Hello, <%= request.getParameter("name") %></h1>
    <p>Current time: <%= new java.util.Date() %></p>
</body>
</html>
```

## JSP Directives
```jsp
<%@ page import="java.util.*, java.text.*" %>
<%@ page errorPage="error.jsp" %>
<%@ page session="true" %>
<%@ page buffer="8kb" %>

<%@ include file="header.jsp" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

## JSP Scriptlets
```jsp
<%
    String name = request.getParameter("name");
    if (name != null) {
        out.println("Hello, " + name);
    }
%>
```

## JSP Expressions
```jsp
<p>Name: <%= user.getName() %></p>
<p>Age: <%= user.getAge() %></p>
<p>Total: <%= calculateTotal() %></p>
```

## JSP Declarations
```jsp
<%!
    private int count = 0;
    
    public int getCount() {
        return ++count;
    }
%>
```

## JSP Standard Actions
```jsp
<!-- Include -->
<jsp:include page="header.jsp" />

<!-- Forward -->
<jsp:forward page="welcome.jsp" />

<!-- Use Bean -->
<jsp:useBean id="user" class="com.example.User" scope="session">
    <jsp:setProperty name="user" property="name" value="John" />
</jsp:useBean>

<jsp:getProperty name="user" property="name" />
```

## JSTL (JSP Standard Tag Library)
```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<!-- If -->
<c:if test="${user.age >= 18}">
    <p>Adult user</p>
</c:if>

<!-- ForEach -->
<c:forEach var="item" items="${items}">
    <p>${item.name}</p>
</c:forEach>

<!-- Choose -->
<c:choose>
    <c:when test="${user.role == 'admin'}">
        <p>Admin</p>
    </c:when>
    <c:when test="${user.role == 'user'}">
        <p>User</p>
    </c:when>
    <c:otherwise>
        <p>Guest</p>
    </c:otherwise>
</c:choose>
```

## EL (Expression Language)
```jsp
<!-- Access properties -->
${user.name}
${user.address.city}

<!-- Access collections -->
${users[0].name}
${map.key}

<!-- Operators -->
${age > 18 ? "Adult" : "Minor"}
${empty list}
${not empty user}
```

## Best Practices
1. Minimize scriptlets (use JSTL/EL)
2. Use MVC pattern
3. Separate business logic
4. Use custom tags
5. Handle errors properly
6. Use appropriate scopes
7. Avoid Java code in JSP
8. Use EL for expressions

