![website (1)](https://github.com/yoogus/ProjectDBCompany/assets/110523242/7b96cc07-7e2b-4926-804f-e6182caebecc) 
#Проект базы данных "Компания"
---
### ![website (1)](https://github.com/yoogus/ProjectDBCompany/assets/110523242/7b96cc07-7e2b-4926-804f-e6182caebecc) 📊В базе данных указаны сотрудники, их компания, личные проекты, задачи к проектам, система заказов и триггеры на автоматизацию.
![image](https://github.com/yoogus/ProjectDBCompany/assets/110523242/ceff3e9c-eb64-42d1-bcec-57e3213b368b)

---
### 📚 Пример триггеров

- Отслеживает изменения в таблице Task и при изменении\добавлении\удаление задач изменения будут записаны в таблицу TaskProjectHistory
` ALTER TRIGGER [dbo].[trg_TaskProjectHistory]
ON [dbo].[Task]
AFTER INSERT, UPDATE
AS
BEGIN
    DECLARE @action nvarchar(50)
    IF EXISTS(SELECT * FROM inserted) AND NOT EXISTS(SELECT * FROM deleted)
        SET @action = 'Добавление'
    ELSE IF EXISTS(SELECT * FROM inserted) AND EXISTS(SELECT * FROM deleted)
        SET @action = 'Изменение'
        
    INSERT INTO [Company].[dbo].[TaskProjectHistory] (Action, TaskID, ProjectID, Description, Status)
    SELECT @action, inserted.ID, inserted.ProjectID, inserted.Description, inserted.Status
    FROM inserted
END `
