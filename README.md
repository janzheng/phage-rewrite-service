# Phage Rewrite Service


This is a small tool used by Phage Directory Network as a redirect/rewrite service for subdomains and custom domains. We do this because moving DNS for Phage Directory is annoying, and I also like that discovery.phage.directory is the subdomain in itself. 

Uses: https://vercel.com/docs/cli#project-configuration/rewrites 
How Super.so does it with Vercel: https://super.so/docs/adding-a-domain 

The way it works is:
- phage.pub DNS servers are set to Vercel
- *.phage.pub allows and routes wildcard domains
  - e.g. randompage.phage.pub
- wildcard domains are rewritten into the PDN subdomain (discovery.phage.directory/randompage)
- this allows randompage.com > randompage.phage.pub > discovery.phage.directory/randompage
