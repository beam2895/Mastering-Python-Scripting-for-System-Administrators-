node ('slave1') {

  def compiled = true
  stage('Source') {
      cleanWs()
    //
    def object =  new JsonSlurper().parseText("$payload")
    // reading the JSON to see if branch is master or any other
    String refs= new JsonBuilder("$object.ref" ).toPrettyString()
    println "$refs"
    String[] arrBranch =refs.split("/")
    String branch =arrBranch[2]
    println "$branch"
    if (branch=="master")
        echo "deploy to PROD"
    else
    echo "deploy to staging"
    //
      dir ('build') {
    echo "Source"  
    git 'https://github.com/beam2895/Mastering-Python-Scripting-for-System-Administrators-'
      }
  }
      try{
    stage('Compile and test') {
    echo "Build stage" 
//    sh 'python --version;sh "find build -iname '*.py''
    
    python_files = sh (
    script: " python -m compileall build ",
    returnStdout: true
).trim()
  }
      } catch(e) {
              compiled = false
 //         echo "python files== ${python_files}"
          ansiColor('vga') {
             echo '\033[42m\033[97mSome files are not compiles, see below error logs \033[0m'
                       echo e.toString()
              
          }

    if(compiled) {
        currentBuild.result = "SUCCESS"
    } else {
        currentBuild.result = "UNSTABLE"
    }

    }
    stage('Build') {
    echo "Post build stage"  
  }
}

