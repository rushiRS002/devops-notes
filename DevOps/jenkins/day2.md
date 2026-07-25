Jenkins Pipeline 

types: 
1. Declarative Pipeline
2. Scripted Pipeline

Declarative Pipeline:
```
pipeline {
    agent {label 'agent'}
        stages {
            stage("pull") {
              steps { 
                echo "pulling code from git"
                git branch: 'main'
                url: 'https://github.com/mukundDeo9325/Project-InsureMe1.git'
              }
            }
            stage("build") {
              steps {
                echo "building the code"
              }
            }
            stage("test") {
              steps {
                echo "testing the code"
              }
            }
            stage("deploy") {
              steps {
                echo "deploying the code"
              }
            }
        }
    }


```

Scripted Pipeline:
```
node {
    stage("pull") {
        echo "pulling code from git"
        git branch 'main',
        url: 'https://github.com/mukundDeo9325/Project-InsureMe1.git'
    }
```
---

## 1. Install Pipeline Plugin

The Pipeline plugin is usually **pre-installed** in Jenkins via the "Suggested Plugins" setup. To verify or install manually:

### Steps

1. Go to **Jenkins Dashboard → Manage Jenkins → Manage Plugins**
2. Click the **Available** tab
3. Search for **"Pipeline"**
4. Check the box next to **Pipeline** *(also install **Pipeline: Stage View** for visualization)*
5. Click **Install without restart** or **Download now and install after restart**
6. Verify under the **Installed** tab after restart


## 2. Scripted Pipeline vs Declarative Pipeline
<img width="843" height="565" alt="Screenshot 2026-07-24 at 1 58 26 PM" src="https://github.com/user-attachments/assets/a9c045e9-de71-48d3-b7dd-18e1e77e77b0" />
- https://www.geeksforgeeks.org/devops/understanding-jenkins-ci-cd-pipeline-and-its-stages/

| Feature | Scripted Pipeline | Declarative Pipeline |
|---|---|---|
| **Syntax** | Groovy-based, full flexibility | Structured, predefined schema |
| **Introduced** | Jenkins 2.0 (original) | Later (recommended standard) |
| **Starting block** | `node { }` | `pipeline { }` |
| **Learning curve** | Steeper | Easier / beginner-friendly |
| **Error handling** | `try/catch/finally` | `post { }` block |
| **Conditionals** | Full Groovy `if/else` | `when { }` directive |
| **Validation** | Runtime errors only | Pre-run syntax validation |
| **Best for** | Complex logic, dynamic flows | Standard CI/CD pipelines |

### Scripted Pipeline Example

```groovy
node {
    stage('Build') {
        echo 'Building...'
    }
    stage('Test') {
        echo 'Testing...'
    }
}
```

### Declarative Pipeline Example

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

> ✅ **Recommendation:** Use **Declarative Pipeline** unless you need advanced dynamic logic.

---

## 3. Basic 4-Stage Pipeline

A standard CI/CD pipeline with **Build → Test → Package → Deploy** stages:

```groovy
pipeline {
    agent any
    stages {

        stage('Pull') {
            steps {
                echo '⬇️ Pulling source code from Git...'
              
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building...'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying...'
            }
        }

    }
}
```

---

## 4. Add Pull Stage

The **Pull stage** is the **first stage** of the pipeline — it pulls source code from a Git repository before building.

---

### Option A — Using `git` Step *(Simple)*

```groovy
pipeline {
  agent any
    stages {
        stage('Pull') {
            steps {
                echo '⬇️ Pulling source code from Git...'
                git branch: 'main',
                    url: 'https://github.com/mukundDeo9325/terraform.git'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building...'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying...'
            }
        }
    }
}
```
## with using agent 
```groovy
pipeline {
  agent { label 'project'}   ## project is an agent label 
    stages {
        stage('Pull') {
            steps {
                echo '⬇️ Pulling source code from Git...'
                git branch: 'main',
                    url: 'https://github.com/mukundDeo9325/terraform.git'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building...'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying...'
            }
        }
    }
}
```
---





