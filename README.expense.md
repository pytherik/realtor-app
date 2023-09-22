# 1

Globally only once on the computer
```bash
$ sudo npm install -g @nestjs/cli
$ nest -v # check installation by getting version number
```
Create new project
```bash
$ nest new <project-name>
```
Start the app
```bash
$  npm run start:dev
```

### nestjs entities:
> every entity is a class
- controllers
  - creating endpoints
- modules
- services
- guards
- data transfer objects
- ...

## Controller
needs a decorator @Controller  
```js
import { Controller, Get } from '@nesjs/common';

@Controller('endpoint')
export class AppController {
  // is route '/endpoint/route'
  @Get('route') 
  getMethod () {
    return 'something'
  }
}
```
business logic of a  
`app.controller.ts`  
gets separated into  
`app.service.ts`  
the information about how the dependencies  
are managed is contained in  
`app.module.ts`  

## Service
> Services hold business logic of Controllers.  
> Their classes are @Injectable: they get injected  
> into the controllers and provide their methods  
> So it can be reused in different controllers needing  
> the same jobs to be done
## Module
> Modules manage the dependencies in the application.  
> @Module contains imports, controllers and providers (i.e. the services).  
> Everything in here is now available in all files of the folder.
## uuid

```bash
$ npm i uuid @types/uuid
```

## Pipe
> Pipes check incoming request parameters  
> if they are parsable to a specific type  
## dto
> data transfer object  
> transforms and validates the body and   
> the outgoing response in the **Controller**.

install packages: libraries for validation  
and transforming decorators  
```bash
$ npm i class-validator class-transformer
```
```js
// ./src/dto/report.dto.ts
import { IsNotEmpty, IsNumber, IsPositive, IsString } from 'class-validator';

export class CreateReportDto {
  @IsOptional()
  @IsString()
  @IsNotEmpty()
  source: string;

  @IsOptional()
  @IsNumber()
  @IsPositive()
  amount: number;
}
```
Validation needs to be implemented globally in  
`./src/main.ts`
```js
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe({
    whitelist: true, // default is false!
  })); // <-
  await app.listen(3000);
}
bootstrap();
```
> the option whitelist makes sure that only defined fields  
> are going through  

### typedeclaration for @IsOptional()

```js
// ./src/data
export interface OptionalType {
  amount?: number;
  source?: string;
}
```
