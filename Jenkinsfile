 def url = BUILD_URL
 def folder = url.tokenize("/")
 def job = " "
for(int i=0;i<folder.size();i++){
    if(folder[i] == "job"){
        job =  folder[i+1]
      
    }
}

 
 def fan = (folder[folder.size()-1] + "/" + job)

def file = [ "master" : "dev"]
def file2= file[env.branch]
print(file2)
node{
    
    stage('invoking pipeline'){
        build job: "private-${file[env.branch]}", propagate: true, wait: true
    }   
    
    stage('download file'){
         
         
         withCredentials([usernamePassword(credentialsId: '2', passwordVariable: 'password', usernameVariable: 'username')]) {

              if (env.yes){
                // def version =  sh( script: 'curl  -H "Accept: application/vnd.github.VERSION.raw" -H "Authorization: Bearer $password" https://api.github.com/repos/$username/pricinto/contents/WebVersion.txt?branch=dev > webversion.txt && jq -r .version webversion.txt > file4.txt && cat file4.txt ',returnStdout: true)
                  def version = sh(returnStdout: true, script: "curl  -H \"Accept: application/vnd.github+json\" -H \"Authorization: Bearer $password\" https://api.github.com/repos/sumitbhardwajji/pricinto/contents/app-${file[env.branch]}.txt?ref=${file[env.branch]} | jq -r .content | base64 -d | jq -r .version ")
                  print(version)
              }   
             
             print "${BUILD_URL}/console"
//              def url = $BUILD_URL
//              print(url)
          }

        }
    stage('commiting'){
         
         withCredentials([usernamePassword(credentialsId: '2', passwordVariable: 'password', usernameVariable: 'username')])
        {
            sh '''
                mkdir -p doc
                cd doc
                rm -dr pricinto
                git clone -b master https://github.com/sumitbhardwajji/pricinto.git
                cd pricinto
                git config --global user.name "sumitbhardwajji"
                git config --global user.email "sumitbhardwa7303@gmail.com"
                echo "sumit bhardwaj" > sumit2.txt
                git add .
                git commit -m "file"
                git push https://sumitbhardwajji:$password@github.com/sumitbhardwajji/pricinto.git --all
                
                '''
        }
        }
         stage("invoking test1"){
         
         build job: 'test1', parameters: [string(name: 'foldername', value:"${fan}")], wait: false
     }
        
    }

