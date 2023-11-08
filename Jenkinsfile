properties([githubProjectProperty(displayName: '', projectUrlStr: 'https://github.com/avielb/rmqp-example')])
node("prod") {
    stage("Checkout"){
        checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/avielb/rmqp-example']])
    }
    stage("Build Producer"){
        bat "docker compose build producer"
    }
    stage("Build Consumer"){
        bat "docker compose build consumer"
    }
    stage("push Producer"){
        bat "docker compose push producer"
    }
    stage("Push Consumer"){
        bat "docker compose push consumer"
    }
    stage("Run"){
        bat "docker compose up -d"
    }
    stage("Test"){
        def Test_result = bat "python e2e.py"
        if(Test_result.equals(-1)){
            error "The test failed! score is not between 0 to 1000"
        }
    }
    stage("Finalize"){
        bat "docker stop localpipelineforscore-score_server-1"
    }
}