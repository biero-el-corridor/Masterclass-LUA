
# LUA FOR CYBER  
  
How to make new out of old.  
  
---
# What is lua ?  
- Created in 1993 by  
	- Roberto Ierusalimschy  
	- Waldemar Celes  
	- Luiz Henrique  
	- Brazil  
- Released as open source in 1994  
  
- First major version (1.0) released in 2003  
- Version 5.4 was released in 2021  
---
# Why is LUA ?  
  
- Easy to learn  
- Data structure == table  
- Table == Data strucutre  
- Flexible  
- Functional OOP  
- compiler is about 400Ko  
- work from Solaris to FreeBSD  
- fast , because c is speed  
- from world of warcraft    
- to wireshark  
- The arch linux of programing language. 
---
# LUA 101  
  
```lua  
     and       break     do        else      elseif    end  
     false     for       function  goto      if        in  
     local     nil       not       or        repeat    return  
     then      true      until     while  
```  
  
```lua  
local lua,better = "hello", "world"  
print(lua,better)  
```  
```lua  
print(type("What is my type"))   --> string  
print(type(5.8*t))               --> number  
print(type(true))                --> boolean  
print(type(print))               --> function  
print(type(nil))                 --> nil  
print(type(type(ABC)))           --> string  
```  
```lua  
myObject = {  
	function if_and_loop(lua,better)  
		--if statement  
		if lua == 10 then print("lua is 10") end  
		--for counter = beginning, end, step do  
		for better=1,10 do print(i) end  
		--while loop  
		while lua < 20 do lua = lua + 1 end  
		   return lua + better, better+lua  
	end  
}  
```  
---
# LUA 102  
  
```lua  
--sample table initialization  
mytable = {}  
--simple table value assignment  
mytable[1]= "Lua"  
--removing reference  
mytable = nil  
-- lua garbage collection will take care of releasing memory  
```  
```lua  
-- pairs  
pokemon = { "Pikachu", 2, 1.337}  
for name, pokemon in pairs(pokemon) do  
	print(name .. " is a " .. pokemon)  
end  
--Pikachu is a Pikachu  
--Charizard is a 2  
--Snorlax is a 1.337  
-- ipairs  
for i, pokemon in ipairs(pokemon) do  
	print("The number " .. i .. " is " .. pokemon)  
end  
--The number 1 is Pikachu  
--The number 2 is 2  
--The number 3 is 1.337  
```  
---
# NMAP NSE  
  
#### General informations  
- 700 + official [script](https://nmap.org/nsedoc/scripts/)  
- 40 + official [lib](https://nmap.org/nsedoc/lib/)  
- active comunity (log4j scanner in D+4)  
- need to be in defined folder  
- need to be standardised  

---
# NMAP NSE  
  
   
- standardised sctructure  
- necessary to be added to the nse offical list  
  
```lua  
description = [[Hack the NSA]]    
 ---  
 -- @usage  
 -- nmap --script ./hack_nsa.nse 8.8.8.8  
 -- @output  
 -- PORT    STATE SERVICE  
 -- 666/tcp open  NSA deep state  
 -- | NSA  
 -- | secret informations:  
 -- |    a3VicmljayBtYWRlIHRoZSBtb29uIGxhbmRpbmcu  
 ---  
author = "biero el corridor"    
license = "MIT"  
-- categories field  
categories = {"default", "discovery", "safe"}  
  
```  
---
# Exemple 1  
  
Simple IP spoofing.  
```lua  
description = [[Simple IP spoofing with nmap]]    
author = "biero el corridor"    
license = "Same as Nmap--See [http://nmap.org/book/man-legal.html](http://nmap.org/book/man-legal.html)"    
categories = {"safe", "discovery"}  
-- define varable  
local ip = "127.0.0.1"  
local port = 80  
function main(data)  
-- send get requests  
local req = http.get(ip, port, {  
method = "GET"  
})  
-- get requests if code == 200  
if req.status == 200 then  
return req.body  
else  
return nil  
end  
end  
```  
  
---
# Exemple 2.1  
  
study of a more complexe script: [ftp-anon]([https://nmap.org/nsedoc/scripts/ftp-anon.html)  
  
```lua  
require "ftp"  
require "shortport"  
```  
  
```lua  
portrule = shortport.port_or_service({21,990}, {"ftp","ftps"})  
```  
```lua  
  local socket, code, message, buffer = ftp.connect(host, port, {request_timeout=8000})  
  if not socket then  
    stdnse.debug1("Couldn't connect: %s", code or message)  
    return nil  
  end  
  if code and code ~= 220 then  
    stdnse.debug1("banner code %d %q.", code, message)  
    return nil  
  end  
```  
---
# Exemple 2.1  
Study of a more complexe script  
```lua  
  local status, code, message = ftp.auth(socket, buffer, "anonymous", "IEUser@")  
  if not status then  
    if not code then  
      stdnse.debug1("got socket error %q.", message)  
    elseif code == 421 or code == 530 then  
      -- Don't log known error codes.  
      -- 421: Service not available, closing control connection.  
      -- 530: Not logged in.  
    else  
      stdnse.debug1("got code %d %q.", code, message)  
      return ("got code %d %q."):format(code, message)  
    end  
    return nil  
  end  
```  
  
- not really lua , but NSE  
- based on lib like python  
  
---
# OLD VS NEW  
  
| xxxxxxxxxxxxxxxxxx | naabu + nuclei | nmap + nse |  
| ------------------ | -------------- | ---------- |  
| supported protocol | 24             | 450+       |  
| language           | yaml           | lua nse    |  
| based language     | go             | c          |  
  
If you whant speed the new stuff is made for you  
  
but if you want versatility or if you have a situation that involves protocol or unconventional situations, the old is for you  

---
## Wireshark Dissector  
  
#### General informations  
- Is used to parse protocols not supported by wireshark.  
- Can be used for euristic network analysis  
  
---
## standarised structure 1.1  
  
In order to write a script, the following elements are necessary (for the simpliest script).  
  
- definition of source and destination ports.  
```lua  
local DOOM = DissectorTable.get("tcp.port")  
modbus:add(666, modbus1_protocol)  
```  
- Protocol creations  
```lua  
doom_protocol = Proto("DOOM666", "DOOM666 .")  
```  
- Definition of fields (size and type source and dest).  
```lua  
health = ProtoField.uint16("Modbus1.health" , "health" , base.DEC)  
```  
---
## standarised structure 1.2  
- applications of the field in the protocol  
```lua  
doom_protocol.fields = { health,x,y,z }  
```  
- Allocation of the fields according to their place in the buffer  
```lua  
 "the buffer corresponde to the data or payload content"  
 "here whe selecte the byte between 6 and 8"    
Subtree:add(health ,buffer(6,2))  
```  
  
Here are the main steps in the logic of the dissector.  
  
---
# Exemple 1.1  
  
![[11.png|900x500]]  
  
---
# Exemple 1.2  
UMAS Schneider  Protocol.    
Header modbus.  
  
![[12.png|900x300]]  
  
---
# Exemple 1.3  
We now have a clear [breakdown](https://github.com/biero-el-corridor/Wireshark-UMAS-Modicon-M340-protocol/blob/main/modbus-umas-schneider.lua) of our packet.  
  
![[13.png]]  
  
---
## Is the dissector still relevant?  
  
  
![[14.PNG]]  
  
---
# Conclusion  
  
- The lua base scripting is still relevant.  because some key software use it.    
- lua is like ruby , nobody mind them, until they know metasploit is writen in ruby, so its the same for nse and  the wireshark dissector.  
  
---
# Ressource  
This presentation is written in markdown and supported by obsidian. Support for presentations is available on my github.  
  
NSE  
  
- [doc NSE](https://nmap.org/book/nse.html)  
- [lib NSE](https://nmap.org/nsedoc/lib/)  
- [Script NSE](https://nmap.org/nsedoc/scripts/))  
  
Wireshark  
- [wireshark exemple of a basic botnet dissections:](https://www.youtube.com/watch?v=I4nf23HywmI&t=558s&ab_channel=dist67))  
- [basic lua dissector:](https://mika-s.github.io/wireshark/lua/dissector/2017/11/04/creating-a-wireshark-dissector-in-lua-1.html))  
- [Add a protocal ti the dissectior table:](https://www.wireshark.org/docs/wsdg_html_chunked/lua_module_Proto.html))  
- [The holy wireshark dissections tutorial (1-5)](https://mika-s.github.io/wireshark/lua/dissector/2017/11/04/creating-a-wireshark-dissector-in-lua-1.html))
---
GITHUB OF  THE MASTERCLASS
![[qr-code.png]]