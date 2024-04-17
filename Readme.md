# YAML

## Content

[Use cases](#use-cases)

[File extention](#file-extention)

[Plugins](#plugins)

[Intendation](#intendation)

[Data structures](#data-structures)

[Multidocuments](#multidocuments)

[Templates](#templates)

[Python](#python)

[Java](#java)

[Validators and linters](#validators-and-linters)

[Spliting files](#spliting-files)

[Test](#test)

[Redactor visual](#redactor-visual)

## Use cases

YAML (YAML Ain't Markup Language) is a human-readable data serialization format commonly used for configuration files, data exchange between languages with different data structures, and for various types of structured data storage.

use cases for YAML:

Configuration Files (Docker Compose, Kubernetes, properties)

Data Serialization

Configuration Management Tools(Ansible)

Data Exchange

Serverless Frameworks (AWS SAM)

Continuous Integration/Continuous Deployment (CI/CD) (CI/CD Pipeline Configuration) (github actions, gcp, aws, gitlab)


## File extention

File Extension the recommended is `.yaml` but sometimes used `.yml`

## Plugins

 YAML Support for VS Code
 
  yaml-mode for Emacs
  
  drawspaces for Gedit.

[Content](#content)


## Intendation

It's very important to use whitespace or tabs(something one)

Tabs are forbidden as indentation.

You can freely choose the number of spaces for indentation. 

More than 8 probably don't make sense, and also this might be a hard limit in the next YAML version.

You can use different indentation inside of the same YAML document, as long as it is the same for one level.

The recommended number of spaces is 2

```yaml

# number
number: 5 


# object
person:
  name: "Vasyl"
  age: 32
  

#Nested sequences can be formatted in a compact way:
sequence:
- level one
- - level two

```
[Content](#content)

## Data structures


```yaml

# number
number: 5 



# also number but float
number: 4.3

# boolean

valid: false
valid: no
valid: off

invalid: true
invalid: yes
invalid: on

# !!!best practice to use only  true/false


# string

city: Kyiv
city: 'Kyiv'
city: "Kyiv"

# !!!best practice to use only city: 'Kyiv' or city: "Kyiv"

# string with special caracters

line: "aaa\nbbb"

#!!!!!YAML has a number of special characters you cannot use in unquoted strings: ` [] {} : > | `. 
#You should quote all strings that contain any of the following characters the following way:
special-characters: "[all] {quoted} :like> this|"



# Null values
foo: ~
bar: null

# comment

# !!!best practice to comment all non obvious solutions


# object
person:
  name: "Vasyl"
  age: 32
  
# !!!pay attention to space before name and age


#list
ages: [1, 3,5,9,78, -5]
ages:
  - 1
  - 3
  - 5
  - 9
  - 78
  - -5
  

#Nested sequences can be formatted in a compact way:
sequence:
- level one
- - level two

# objects list
people:
  - name: "Ivan"
    age: 21
  - name: "Marina"
    age: 25
  - name: "Oleh"
    age: 73




# multiline text
multilineText: "line 1\nline 2\n....line n"

# alternative to use pipe |
multilineText: |
 line 1
 line 2
 ....
 line n


# long string
singlelineText: >
  begin
  ...
  continue same line
  ...
  end
 

 
 # dictionary is a collection of key: value mappings. All keys are case-sensitive
 best-jedi: {name: Obi-Wan, side: light} 

 jedi:
  name: Obi-Wan Kenobi
  home-planet: Stewjon
  species: human
  master: Qui-Gon Jinn
  height: 1.82m
  
# Dictionaries can be nested in lists (and vice versa) to create more complex structures:
  
  requests:
  # first item of `requests` list is just a string
  - http://example.com/
 
  # second item of `requests` list is a dictionary
  - url: http://example.com/
    method: GET
	

# Tags

# define type string 
not-date: !!str 2022-01-12

# define user type
myCat: !Cat { name: Tom, color: red }


# References
# where & is a link
# where * is a value

references:
  val1: &ref This will be repeated
  val2: *ref
```
[Content](#content)

## Multidocuments

In one file can be a few documents the 3-(----) is syntax to split documents
```yaml

---
player: playerOne
action: attack (miss)
---
player: playerTwo
action: attack (hit)
---
```
[Content](#content)

## Templates

After creating template we can replace <service_name>  and other by using utils like sed

```yaml 
apiVersion: apps/v1
kind: Deployment 
metadata:
  name:  <service_name> 
  labels:  
    app: <service_name>
spec:
  replicas: 3
  template:
    metadata: 
      labels:
        app: <service_name>                
    spec:
      containers:
      - name: <service_name> 
        image: <image>:<tag>
        ports:
        - containerPort: <port> 
---        
apiVersion: v1
kind: Service
```

### Linux

```sh
export REGION="us-west1"
export ZONE="us-west1-c"
for file in sample-app/cloudbuild-dev.yaml sample-app/cloudbuild.yaml; do
sed -i "s/<your-region>/${REGION}/g" "$file"
sed -i "s/<your-zone>/${ZONE}/g" "$file"
done

sed -i "s/<version>/v1.0/g" cloudbuild-dev.yaml

IMAGE=us-east4-docker.pkg.dev/qwiklabs-gcp-01-2cff279e8a65/my-repository/hello-cloudbuild-dev:v1.0

sed -i "s#<image>#$IMAGE#g" dep.yaml
```

### NOTE!!!!!!!!!!!!!!!!!

why sed -i "s#<todo>#$IMAGE#g" works and sed -i "s/<todo>/$IMAGE/g" doesn't work

If the IMAGE variable contains forward slashes /, they will interfere with the sed command's pattern delimiter, which is also a forward slash /.

Using a different delimiter, such as #, avoids this issue and allows the sed command to work correctly.



### Windows 

```powershell
$IMAGE="us-east4-docker.pkg.dev/qwiklabs-gcp-01-2cff279e8a65/my-repository/hello-cloudbuild-dev:v1.1"
(Get-Content "dep.yaml").Replace('<image>', $IMAGE) | Set-Content "dep.yaml"

```



[Content](#content)


## Python



pip install pyyaml

### Read

```python
import yaml
from pprint import pprint

with open('info.yaml') as f:
    templates = yaml.safe_load(f)

pprint(templates)
```

### Write

```python
import yaml


data = {
  "server": {
    "port": 8000,
    "enabled": true
  },
  "clients": [
    {"name": "Client1", "address": "192.168.1.100"}, 
    {"name": "Client2", "address": "192.168.1.101"}
  ]
}

with open('sw_temp.yaml', 'w') as f:
    yaml.dump(data, f)

with open('sw_temp.yaml') as f:
    print(f.read())

```
[Content](#content)


## Java

```xml
<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>2.2</version>            
</dependency>

```

Replace "path/to/your/file.yaml" and "path/to/your/output.yaml"

### Read

```java
import org.yaml.snakeyaml.Yaml;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Map;

public class ReadYamlExample {
    public static void main(String[] args) {
        Yaml yaml = new Yaml();
        
        try {
            FileInputStream fis = new FileInputStream("path/to/your/file.yaml");
            
            // Parse YAML file into a Map
            Map<String, Object> obj = yaml.load(fis);
            
            // Print the Map
            System.out.println(obj);
            
            // Access specific fields
            String apiVersion = (String) obj.get("apiVersion");
            System.out.println("apiVersion: " + apiVersion);
            
            Map<String, Object> spec = (Map<String, Object>) obj.get("spec");
            int replicas = (int) spec.get("replicas");
            System.out.println("replicas: " + replicas);
            
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```


### Write

```java
import org.yaml.snakeyaml.DumperOptions;
import org.yaml.snakeyaml.Yaml;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class WriteYamlExample {
    public static void main(String[] args) {
        Yaml yaml = new Yaml();
        
        // Create a Map to represent the YAML content
        Map<String, Object> data = new HashMap<>();
        data.put("apiVersion", "apps/v1");
        
        Map<String, Object> spec = new HashMap<>();
        spec.put("replicas", 3);
        
        Map<String, Object> selector = new HashMap<>();
        Map<String, Object> matchLabels = new HashMap<>();
        matchLabels.put("app", "dev-app");
        selector.put("matchLabels", matchLabels);
        
        Map<String, Object> template = new HashMap<>();
        Map<String, Object> metadata = new HashMap<>();
        Map<String, Object> labels = new HashMap<>();
        labels.put("app", "dev-app");
        metadata.put("labels", labels);
        
        Map<String, Object> containers = new HashMap<>();
        containers.put("name", "dev-container");
        containers.put("image", "us-east4-docker.pkg.dev/qwiklabs-gcp-01-2cff279e8a65/my-repository/hello-cloudbuild-dev:v1.1");
        
        template.put("metadata", metadata);
        template.put("spec", containers);
        
        spec.put("selector", selector);
        spec.put("template", template);
        
        data.put("spec", spec);
        
        // Write the Map to a YAML file
        try (FileWriter writer = new FileWriter("path/to/your/output.yaml")) {
            yaml.dump(data, writer);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


```
[Content](#content)


## Validators and linters

yamllint — linter YAML, can be integrated in CI/CD pipelines.

drona — validator for security. find credentials

kubeval - validator for Kubernetes (deployments, services). 

[Content](#content)



## Spliting files


```yaml
mysql:
  image: mysql:8.0
  ports:
    - 3306:3306  
  environment:
    - MYSQL_ROOT_PASSWORD=passwd
    - MYSQL_DATABASE=appdb 
  volumes:
    - mysql-data:/var/lib/mysql
volumes:
  mysql-data:

```

can split to mysql-creds.yml:

```yaml  
&default_conn
user: 'root'
pass: 'passwd'  
db: 'appdb'
```

and main.yaml:

```yaml
mysql:
  image: mysql:8.0
  ports:
    - 3306:3306  
  environment:
    MYSQL_ROOT_PASSWORD: *default_conn.pass
    MYSQL_DATABASE: *default_conn.db 
  volumes:
    - mysql-data:/var/lib/mysql
volumes:
  mysql-data:

```
[Content](#content)

## Test  

```python
import yaml
import pytest

def test_config_has_variables(yaml_config):
  
  config = yaml.safe_load(yaml_config)  
  
  # Check properties 
  assert config['app']['version'] == '1.3' 
  assert config['database']['port'] == 3306
  
  # More assertions of config schema...
```

[Content](#content)

## Redactor visual

https://kui.tools

https://codebeautify.org/yaml-editor-online

[Content](#content)


