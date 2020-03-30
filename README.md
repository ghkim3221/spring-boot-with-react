# Spring Boot with React

Spring Boot + React

## Usage

JUST `./gradlew build`, `./gradlew bootJar` or `./gradlew bootRun`.

## Modules

- `bacnekd`: A Spring Boot application to serve static files built from `frontend` module.
- `frontend`: A React application created using the [create-react-app](https://github.com/facebook/create-react-app).

## Builds

The `frontend` module exposes two tasks, `clean` and `build`.

- `clean`: Delete `build` directory generated by `npm run build`.

  ```gradle
  task clean {
    group = 'build'

    delete('build')
  }
  ```

- `build`: Call `clean` task, and `npm run build`.

  ```gradle
  task build {
    group = 'build'

    dependsOn('clean')
    dependsOn('npm_run_build')
  }
  ```

The `backend` module's `ProcessResources` depends on `frontend`'s `build` task.
Then, generated files are moved into `static` resource directory.
They will be served by the Spring.

```gradle
tasks.withType(ProcessResources) {
    dependsOn(':frontend:build')

    from(file(Paths.get(project(':frontend').projectDir.absolutePath, 'build'))) {
        into 'static'
    }
}
```

## License

```
MIT License

Copyright (c) 2020 Gihwan Kim

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
