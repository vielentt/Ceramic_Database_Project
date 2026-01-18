# Создание базы данных для магазина керамики 

Для анализа бизнес-процесса продажи керамических изделий были разработаны IDEF0, DFD, IDEF3 модели. Построена база данных на SQL, где хранится информация о товарах на складе, клиентской базе, информация о заказах, сотрудниках и доставке товара.

<details>
<summary> 1. Моделирование и анализ бизнесс-процесса. Создание IDEF0, DFD, IDEF3 диаграмм.</summary>

Магазин керамики специализируется на горшках и кашпо для растений, но так же предлагает горшки и капо из других материалов (для расширения бизнеса).

![IDEF0](/images/Ceramic_idef0.png)
![DEC1](/images/Ceramic_decomposition.png)
![DEC2](/images/Ceramic_decomposition_2.png)

Для создания диаграмм использовался ERwin Process Modeler. Файл с моделями: [`Ceramic_model.erwin`](/Ceramic_model.bp1)

</details>

<details>
<summary> 2. Проектирование информационной системы на языке UML</summary>

![use-case](/images/Ceramic_use_case.png)
![sequence](/images/Ceramic_Sequence.png)
![collaboration](/images/Ceramic_Collaboration.png)
![class](/images/Ceramic_class.png)
</details>

<details>
<summary> 3. Создание Базы Данных SQL</summary>

![Модель БД](/images/Ceranic_SQL_Workbench.JPG)

Файл БД MySQL Workbench: [`Ceramic_DataBase.mwb`](/Ceramic_DataBase.mwb)

Текст SQL был скопирован в файл [`Ceramic_Sql_Text.txt`](/Ceramic_Sql_Text.txt)
Ниже преведены фрагменты:

**Создание Таблицы сотрудников**
```
CREATE TABLE IF NOT EXISTS `mydb`.`Сотрудник` (
  `Идентификатор Сотрудника` INT NOT NULL AUTO_INCREMENT,
  `Фамилия` VARCHAR(50) NOT NULL,
  `Имя` VARCHAR(50) NOT NULL,
  `Отчество` VARCHAR(50) NULL,
  `Дата Рождения` VARCHAR(50) NOT NULL,
  `Данные паспорта` VARCHAR(100) NOT NULL,
  `Идентификатор отдела` INT NOT NULL,
  `Рабочий телефон` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`Идентификатор Сотрудника`),
  UNIQUE INDEX `idEmployee_UNIQUE` (`Идентификатор Сотрудника` ASC) VISIBLE,
  UNIQUE INDEX `Passport_UNIQUE` (`Данные паспорта` ASC) VISIBLE)
ENGINE = InnoDB;
```
**Создание таблицы с ячейккми склада**
```
DROP TABLE IF EXISTS `mydb`.`Склад` ;

CREATE TABLE IF NOT EXISTS `mydb`.`Склад` (
  `Идентификатор ячейки товара на складе` INT NOT NULL,
  `Количество товара` INT NULL,
  PRIMARY KEY (`Идентификатор ячейки товара на складе`))
ENGINE = InnoDB;
```
**Создание таблицы моделей горшков**
```
CREATE TABLE IF NOT EXISTS `mydb`.`Горшок` (
  `Идентификатор модели горшка` INT NOT NULL,
  `Идентификатор ячейки товара на складе` INT NOT NULL,
  `Наименование модели горшка` VARCHAR(100) NULL,
  `Материал` VARCHAR(45) NULL,
  `Цвет` VARCHAR(45) NULL,
  `Рисунок` VARCHAR(45) NULL,
  `Форма` VARCHAR(45) NULL,
  `Объём` VARCHAR(45) NULL,
  `Высота` VARCHAR(45) NULL,
  `Наличие поддона` VARCHAR(45) NULL,
  `Глубина поддона` VARCHAR(45) NULL,
  `Количество отверстий` INT NULL,
  PRIMARY KEY (`Идентификатор модели горшка`, `Идентификатор ячейки товара на складе`),
  INDEX `fk_Горшок_Склад1_idx` (`Идентификатор ячейки товара на складе` ASC) VISIBLE,
  CONSTRAINT `fk_Горшок_Склад1`
    FOREIGN KEY (`Идентификатор ячейки товара на складе`)
    REFERENCES `mydb`.`Склад` (`Идентификатор ячейки товара на складе`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;
```
**SELECT для проверки правильности построения бд и заполнения данных**
```
SELECT * FROM `Сотрудник` WHERE `Рабочий телефон` NOT LIKE '+7%' ;
SELECT * FROM `Клиент` WHERE `Идентификатор Сотрудника` != 2;
SELECT `Клиент`.`ФИО`, `Клиент`.`Идентификатор Сотрудника`, `Сотрудник`.`Фамилия`  
FROM `Клиент`, `Сотрудник` WHERE `Клиент`.`Идентификатор Сотрудника`= `Сотрудник`.`Идентификатор Сотрудника`;
```
</details>