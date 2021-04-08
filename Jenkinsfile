/*
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing,
 * software distributed under the License is distributed on an
 * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 * KIND, either express or implied.  See the License for the
 * specific language governing permissions and limitations
 * under the License.
 */
def NODE = 'git-websites'
def STOP_SQUASH_AT = '5570b6e2e57afb5f0a8f78f9bf9c33bd6f16aaed'

pipeline {
    agent {
        label "$NODE"
    }

    options {
        buildDiscarder(
            logRotator(artifactNumToKeepStr: '5', numToKeepStr: '10')
        )

        ansiColor('xterm')

        checkoutToSubdirectory('camel-website')
    }

    environment {
        ANTORA_CACHE_DIR  = "$WORKSPACE/.antora-cache"
        HUGO_CACHEDIR     = "$WORKSPACE/.hugo-cache"
        CAMEL_ENV         = 'production'
    }

    stages {
        stage('Website') {
            agent {
                dockerfile {
                    dir 'camel-website'
                    label "$NODE"
                    reuseNode true
                    args '-u root'
                }
            }

            environment {
                HOME = "$WORKSPACE"
            }

            steps {
                sh "cd $WORKSPACE/camel-website && yarn clean; yarn build-all"
            }
        }

        stage('Checks') {
            agent {
                dockerfile {
                    dir 'camel-website'
                    label "$NODE"
                    reuseNode true
                    args '-u root'
                }
            }

            environment {
                HOME = "$WORKSPACE"
            }

            steps {
                sh "cd $WORKSPACE/camel-website && yarn checks"
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }

            steps {
                dir('deploy/staging') {
                    deleteDir()
                    sh 'git clone -b asf-site https://gitbox.apache.org/repos/asf/camel-website.git .'
                    sh "git -c core.editor='sed -i 2,/\$(git log --skip=9 -1 --pretty=format:%h)/s/^pick/squash/' rebase -q --interactive $STOP_SQUASH_AT | grep -v 'create mode 100644'" // squash all but initial and last 9 commits
                    sh 'git rm -q -r *'
                    sh "cp -R $WORKSPACE/camel-website/public/. ."
                    sh 'git add .'
                    sh 'git commit -m "Website updated to $GIT_COMMIT"'
                    sh 'git push --force origin asf-site'
                }
            }
       }
    }
}
