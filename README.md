# CarRacers
## A Blazor learning project for transitioning from Java to C# development

This is a small playground application. I built it to get comfortable with C# and the Blazor Server framework after working primarily with Java. The application acts as a digital logbook for motorsport events. It manages cars, drivers, and race records. Because I wanted to learn SQL integration in .NET, the system connects to a local MySQL database using a Database-First approach with Dapper as a lightweight micro-ORM. It does basic tracking of vehicle mileage and driver points.

---

## Features & Visualizations

<p>You can add, delete and edit data about car racers.</p>

![image](https://user-images.githubusercontent.com/78794923/181766758-6768365a-8044-44f8-abf4-c18532535533.png)

<p>You can add, delete and edit data about cars.</p>

![image](https://user-images.githubusercontent.com/78794923/181766900-250bf9f9-de95-40e0-866a-554afe69f860.png)

<p>The app supports race data entry, and once the race data is entered, the car's mileage increases.
After entering the race data, the driver's points are incremented.</p>

![image](https://user-images.githubusercontent.com/78794923/181766909-bb771a2e-4d05-4731-9993-6fdfc6501237.png)

<p>There is a ranking of drivers based on the points they have received.</p>

![image](https://user-images.githubusercontent.com/78794923/181766912-4d1b2607-5e70-48dd-a7af-bfa9c541170f.png)

---

## Installation for Contributors

To run this project locally, you need the .NET SDK (version 6.0 or higher) and a MySQL server instance running.

### 1. Database Setup
Create a MySQL database named `carracers` and run the following schema script to create the necessary tables:

```sql
CREATE DATABASE IF NOT EXISTS carracers;
USE carracers;

CREATE TABLE IF NOT EXISTS `driver` (
  `idDriver` INT AUTO_INCREMENT PRIMARY KEY,
  `firstNameDriver` VARCHAR(100) NOT NULL,
  `middleNameDriver` VARCHAR(100),
  `lastNameDriver` VARCHAR(100) NOT NULL,
  `ageDriver` INT NOT NULL,
  `nationalityDriver` VARCHAR(100),
  `pointsDriver` INT DEFAULT 0
);

CREATE TABLE IF NOT EXISTS `car` (
  `idCar` INT AUTO_INCREMENT PRIMARY KEY,
  `modelCar` VARCHAR(100) NOT NULL,
  `brandCar` VARCHAR(100) NOT NULL,
  `mileageCar` INT DEFAULT 0,
  `serviceDate` DATETIME NOT NULL
);

CREATE TABLE IF NOT EXISTS `race` (
  `idRace` INT AUTO_INCREMENT PRIMARY KEY,
  `trackRace` VARCHAR(255) NOT NULL,
  `dateRace` DATETIME NOT NULL,
  `mileageRace` INT NOT NULL,
  `lengthRace` INT NOT NULL,
  `pointsRace` INT NOT NULL
);

CREATE TABLE IF NOT EXISTS `race_has_car_and_driver` (
  `id` INT AUTO_INCREMENT PRIMARY KEY,
  `idRace` INT NOT NULL,
  `idCar` INT NOT NULL,
  `idDriver` INT NOT NULL
);
```

### 2. Connection String Configuration
Update the connection string in `CarRacers/appsettings.json` to point to your local MySQL instance. Adjust the credentials (`Uid` and `Pwd`) as needed:

```json
"ConnectionStrings": {
  "default": "Server=127.0.0.1;Port=3306;Database=carracers;Uid=YOUR_USER;Pwd=YOUR_PASSWORD;"
}
```

### 3. Build and Run
Clone the repository and run the application using the dotnet CLI from the root folder:

```bash
# Clone the repository
git clone https://github.com/yourusername/CarRacers.git
cd CarRacers

# Restore dependencies and build the solution
dotnet build

# Run the Blazor Server application
dotnet run --project CarRacers
```

---

## Known Issues and Future Plans

Currently, the application contains security vulnerabilities, primarily SQL injection risks. Several updates in the Razor views utilize raw string concatenation for passing parameter IDs directly to database scripts instead of using parameterized inputs. Also, there is a lack of architecture separation since the Blazor components query the database wrapper directly, which bypasses any intermediate service layer.

Database constraint management remains incomplete. If you delete racers, vehicles, or events, the system does not cascade delete their relations within the linking table, producing orphan rows in the database. Furthermore, form submissions do not validate user inputs on either the client or server side. Lastly, the Syncfusion script integration disables isolation, which could impact page rendering performance.

