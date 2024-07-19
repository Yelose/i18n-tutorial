# How to use I18n in Angular 18+ standalone

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 18.0.6.

## Internationalization i18n

- Instalación de ngx translate

```
npm install @ngx-translate/core --save
```

```
npm install @ngx-translate/http-loader
```

- Crear dentro de `/public` las carpetas

/assets/i18n

- Crear dentro de i18n cada archivo `.json`

- Añadir el diccionario en `app.config.ts`

```ts
export const appConfig: ApplicationConfig = {
  providers: [provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes),
    provideHttpClient(),
    importProvidersFrom(TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [HttpClient]
      }
    }))]
};
```

- En `app.component.ts` añadir la internacionalización para toda la página.

```ts
  ngOnInit(){
    let language = "en"
    language = localStorage.getItem('language') || 'en'
    this.translate.setDefaultLang(language)
  }
```

- En cada componente que se va a utilizar el pipe `translate`, hay que importar TranslateModule incluído `app.component.ts`

```ts
import { TranslateModule, TranslateService } from '@ngx-translate/core';
```

```ts
  constructor( private translate: TranslateService){}

  switchLanguage(language: string){
    localStorage.setItem("language", language)
    this.translate.setDefaultLang(language)
  }
```

- Uso en el template

```html
<button (click)="switchLanguage('es')">ES</button>
<button (click)="switchLanguage('en')">EN</button>

<p> {{ 'app.welcome' | translate}} </p>
<p> {{ 'app.slogan' | translate}}</p>
<p> {{ 'app.brand' | translate}} </p>
```

- `app.component.ts`

```ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { TranslateModule, TranslateService } from '@ngx-translate/core';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, TranslateModule],
  templateUrl: './app.component.html',
  styleUrl: './app.component.scss'
})
export class AppComponent {
  title = 'i18n';
  constructor( private translate: TranslateService){}

  switchLanguage(language: string){
    localStorage.setItem("language", language)
    this.translate.setDefaultLang(language)
  }

  ngOnInit(){
    let language = "en"
    language = localStorage.getItem('language') || 'en'
    this.translate.setDefaultLang(language)
  }
}
```

- `app.config.ts`

```ts
import { ApplicationConfig, importProvidersFrom, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { TranslateLoader, TranslateModule} from '@ngx-translate/core';

import { routes } from './app.routes';
import { HttpClient, provideHttpClient } from '@angular/common/http';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

export function HttpLoaderFactory(http: HttpClient) {
  return new TranslateHttpLoader(http, './assets/i18n/', '.json')
}

export const appConfig: ApplicationConfig = {
  providers: [provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes),
    provideHttpClient(),
    importProvidersFrom(TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [HttpClient]
      }
    }))]
};

``

- `app.component.html`

```html

<button (click)="switchLanguage('es')">ES</button>
<button (click)="switchLanguage('en')">EN</button>

<p> {{ 'app.welcome' | translate}} </p>
<p> {{ 'app.slogan' | translate}}</p>
<p> {{ 'app.brand' | translate}} </p>

```

- Ejemplo archivos `.json` cutres

es.json

```json
{
  "app": {
    "welcome": "Te damos la bienvenida",
    "slogan": "Slogan de la marca",
    "brand": "Marca"
  }
}

```

en.json

```json
{
  "app": {
    "welcome": "Welcome",
    "slogan": "Company slogan",
    "brand": "Brand"
  }
}

```
