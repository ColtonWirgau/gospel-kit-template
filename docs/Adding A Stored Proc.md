# Adding a Stored Procedure

## Naming Convention: 
- api_TheHub_... for shared between customers
- api_TheHub_Custom_... for an SP that is just for one customers

## Security Best Practices
- We will not, by and large, pass a @UserName parameter as Custom Widgets did.  The security of a "dashboard" or "app" in the hub will be from user groups.
