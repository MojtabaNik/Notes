# How to run iis express web project on local server

## 1. Open your project applicationhost.config

applicationhost.config destination: ```'{project root}\.vs\config\applicationhost.config'```

## 2. Add your local IP address with port inside your web project ```<site><bindings> </bindings></site>``` tag

```xml
           <binding protocol="http" bindingInformation="*:port:ip" />
```

```xml
            <site name="WebProjectName" id="*">
                <application path="/" applicationPool="Clr4IntegratedAppPool">
                    <virtualDirectory path="/" physicalPath="PathToYourProject" />
                </application>
                <bindings>
                    <binding protocol="http" bindingInformation="*:2155:localhost" />
                    //You should add top snippet here!
                </bindings>
            </site>
```

## 3. Open CMD or PowerShell as administator and run this command

```cmd
netsh http add urlacl url=http://ip:port/ user=everyone
```