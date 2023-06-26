# Security Operation Components

### 1. SAST Static Application Security Testing/Whitebox Penetration Testing
### 2. DAST Dynamic Application Security Testing
### 3. RASP Runtime Application Security Protection
### 4. IAST  Interactive Application Security Testing
### 5. SCA Software Composition Analysis
### 6. IaC Infrastructure as Code

## Tools:

SAST: SonarQube,Snyk, Veracode
DAST: AppScan, Checkmarx
RASP: Sqreen(Datadog), OpenRASP by Baidu
IAST: Seeker by Synopsys, Contrast Assess
SCA : RetireJs, Safety, Snyc
IaC : Ansible, Chef, Puppet, Terraform, AWS Cloudformation


## Pipeline

### Develop : 
    - Pre-commit hooks, 
    - IDE Plugins
### Code Repository : 
    - Secrets Management
### CICD : 
    - SAST, 
    - SCA
### Build : 
    - Binary Repository
### Staging / QA : 
    - DAST, 
    - Vulnerability Management
### Production: 
    - Compliance as Code 
    - Security in IaC
    - Vulnerability Assessment
    - Penetration Testing
### Monitoring
    - Alerting and Monitoring for OWASP Top 10


Basic Pipeline : 
- Build Test : basic build status of the application
- SCA : with retireJS with we can check the software composition analysis with 
    - `retire --severity high --outputformat json --outputpath <PATH>`
- SCA Safety : It is only specific to python
    - `safety check -r requirements.txt --json | tee safety_output.json`
- test : Unit code execution and any automation test tools need to be in place 
    - `npm run test`
- SAST: 
    - `bandit -r . -f json | tee bandit_sast_output.json`
    - bandit is only useful for python application
- SAST : Secret Check with `trufflehog`
    - `docker run -v $(pwd):/src --rm hysnsec/trufflehog file:///src --json | tee sast_trufflehog_output.json`
- Release Prerequisites: 
    - `docker build -t <IMAGE_NAME> .`
    - `docker push <IMAGE_NAME>`
- Release / Deploy:
    - This is based on the deployment strategy like any orchestration tools or a plain deployment or aws solutions
- DAST - OWASP ZAP 
    - `docker run -u zap -p 8080:8080 -i owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080 -config api.disablekey=true` OR
    - `docker run -u $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable zap-baseline,.py -t http<s>://<$PROD_HOST>:<PORT>   -x dast-zap-output.xml`
- DAST Nikto : 
    - Another tool to scan more features which are not covered as part of the above owasp 
    - `docker run -u $(id -u):$(id -g) -v $(pwd):/nikto/wrk:rw --rm hysnsec/nikto --output dast_nikto_output.json`

- DAST NMAP:
    - Identify the open network ports 
    - `nmap $PROD_HOST -oX nmap_dast_output.xml`

Result Collection Tools:

**No result != no vulnerabilities**

Gremwell Magictree (local)
• Burp (as of Burp Suite version 1.3.07)
• Nmap
• Nikto
• Nessus XML v.1
• Nessus XML v.1 
• Nessus XML v.2
• OpenVAS
• Qualys
• Imperva Scuba
• w3af
• Acunetix
• Rapid 7 NeXpose
• Arachni
• OWASP Zed Attack Proxy
• Metasploit
• IBM Rational AppScan

Dradis Framework (web application)
• Burp Scanner
• Metasploit
• Nessus
• NeXpose
• Nikto
• Nmap
• OpenVAS
• OSVDB
• Retina
• SureCheck
• VulnDB
• w3af
• wXf
• Zed Attack Proxy