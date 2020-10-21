#!/usr/bin/env groovy
// Define variables
List category_list = ['"Select:selected"','"Vegetables"','"Fruits"']
List fruits_list = ['"Select:selected"','"apple"','"banana"','"mango"']
List vegetables_list = ['"Select:selected"','"potato"','"tomato"','"broccoli"']
List default_item = ['"Not Applicable"']

String categories = buildScript(category_list)
String vegetables = buildScript(vegetables_list)
String fruits = buildScript(fruits_list)
String items = populateItems(default_item,vegetables_list,fruits_list)

def onError = [classpath: [], sandbox: false, script: 'return ["ERROR"]']

// Methods to build groovy scripts to populate data
String buildScript(List values){
  return "return $values"
}
String populateItems(List default_item, List vegetablesList, List fruitsList){
return """
     if(SelectedCategories.equals('Vegetables')){
        return $vegetablesList
     }
     else if(SelectedCategories.equals('Fruits')){
        return $fruitsList
     }else{
        return $default_item
     }
     """
}

def createList(String listName, String categories){
       return [$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', name: 'SelectedCategories', 
        script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false, script:  categories]]]
}
// Properties step to set the Active choice parameters via 
// Declarative Scripting
properties([
    parameters([
        createList("SelectedCategories", categories)
        ,
        [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT',name: 'SelectedItems', referencedParameters: 'SelectedCategories', 
        script: [$class: 'GroovyScript', fallbackScript: onError, script: [classpath: [], sandbox: false, script: items]]]
    ])
])

pipeline {
    agent any 
    stages {

        stage("first stage"){
            steps {
                echo "welcome to the ${SelectedCategories}"

            }
        
        }

        stage("Second stage"){
            steps {
                echo "bye bye world ${SelectedItems}"

            }
        
        }
    }

}