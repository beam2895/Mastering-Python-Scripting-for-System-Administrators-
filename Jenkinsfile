import groovy.json.JsonSlurper
node ('slave1') {

  def compiled = true
  stage('Source') {
      cleanWs()
    //
     properties([
  pipelineTriggers([
   [$class: 'GenericTrigger',
    genericVariables: [
     [key: 'ref', value: '$.ref'],
     [
      key: 'before',
      value: '$.before',
      expressionType: 'JSONPath', //Optional, defaults to JSONPath
      regexpFilter: '', //Optional, defaults to empty string
      defaultValue: '' //Optional, defaults to empty string
     ]
    ],
    genericRequestVariables: [
     [key: 'requestWithNumber', regexpFilter: '[^0-9]'],
    ],
    genericHeaderVariables: [
     [key: 'headerWithNumber', regexpFilter: '[^0-9]'],
    ],

    causeString: 'Triggered on $ref',

    token: 'abc123',
    tokenCredentialId: '',

    printContributedVariables: true,
    printPostContent: true,

    silentResponse: false,

    regexpFilterText: '$ref',
    regexpFilterExpression: 'refs/heads/' + BRANCH_NAME
   ]
  ])
 ])
    //
      dir ('build') {
    echo "Source " 
    echo "$ref"
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

