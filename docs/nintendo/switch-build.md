---
icon: lucide/hammer
---

# Nintendo Switch Build

Для сборки билда под платформу Nintendo Switch необходимо выполнить следующие шаги:

> Подробнее в [документации](https://docs.google.com/document/d/1IMWRcmw9Ts8Yf2HYe5PBdM0CyNS6fVQz/edit?usp=sharing&ouid=108020388252141246965&rtpof=true&sd=true).

## Вылет (crash) после первого запуска

Самые частые проблемы:

- **Попытка создать файл**: `Error: nn::fs::CreateDirectory() failed because the operation is unsupported` > **[Рефактор Save System]** (замена `persistentDataPath`, `FileStream`)
- **Превышение используемой RAM**: `Accessed to unmapped memory space` > **[Сжатие текстур]**, чтобы одна сцена занимала не более **2.9 ГБ** или **рефактор** `Loading Manager`: Сцена 1 > Пустая сцена c отгрузкой `Resources.UnloadUnusedAssets()` и `GC.Collect()` > Сцена 2
- **Ошибка загрузки ассетов**: `Application has been terminated manually` > **Асинхронная загрузка** по частям или загрузка без **Addressables**
- **Ссылки на файлы, что были разрушены** или еще не загрузились: `NullReferenceException: Object reference not set to an instance of an object` > Проверка сцен в **Unity Editor** и отладка во время **Play Mode**

[Рефактор Save System]: ../guides/unity.md#saveload-system
[Сжатие текстур]: ../guides/unity.md#_6

??? note "Пример поиска отсутствующих скриптов > Добавить в ..\Assets\Editor"

    ``` CSharp title="FindMissingScripts.cs"
    using UnityEngine;
    using UnityEditor;
    using System.Collections.Generic;
    using UnityEngine.SceneManagement;
    
    public class SelectGameObjectsWithMissingScripts : Editor
    {
        [MenuItem("Tools/Validation Utilities/Select GameObjects With Missing Scripts")]
        static void SelectGameObjects()
        {
            //Get the current scene and all top-level GameObjects in the scene hierarchy
            Scene currentScene = SceneManager.GetActiveScene();
            GameObject[] rootObjects = currentScene.GetRootGameObjects();
    
            List<Object> objectsWithDeadLinks = new List<Object>();
            foreach (GameObject g in rootObjects)
            {
                //Get all components on the GameObject, then loop through them 
                Component[] components = g.GetComponents<Component>();
                for (int i = 0; i < components.Length; i++)
                {
                    Component currentComponent = components[i];
    
                    //If the component is null, that means it's a missing script!
                    if (currentComponent == null)
                    {
                        //Add the sinner to our naughty-list
                        objectsWithDeadLinks.Add(g);
                        Selection.activeGameObject = g;
                        Debug.Log(g + " has a missing script!");
                        break;
                    }
                }
            }
            if (objectsWithDeadLinks.Count > 0)
            {
                //Set the selection in the editor
                Selection.objects = objectsWithDeadLinks.ToArray();
            }
            else
            {
                Debug.Log("No GameObjects in '" + currentScene.name + "' have missing scripts! Yay!");
            }
        }
    }
    ```

!!! warning

    Содержание этого документа является строго конфиденциальным. Обращайтесь с ним соответственно.