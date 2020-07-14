---
title: 🚀Angular🚀 - @ngneat/transloco - multiple scopes merged and lazy loaded
---

[Github markdown version - can be easier to read](https://github.com/yerevin/blog/blob/gh-pages/_posts/2020-07-08-angular-transloco-multiple-scopes-merged-and-lazy-loaded.md)

Requirements: project with default @ngneat/transloco configuration

If you use [@ngneat/transloco](https://github.com/ngneat/transloco) and were wondering how to lazy load multiple scopes inside module there is a solution for you.
So you have your project configured already with @ngneat/transloco and you want to simply load multiple translations lazy loaded in the scope of choosen module.

There are steps that you need to follow to make it work - let's go.

1) Redefine default TransolocoLoader getTranslation function [Transloco Loader](https://github.com/yerevin/hire-me-recruitation-repo/blob/master/angular-rxjs/src/app/transloco.loader.ts)
```typescript
  getTranslation(langPath: string) {
    let [multiplePaths, activeLang] = langPath.split('/');
    let splittedMultiplePaths = multiplePaths.split(',');

    let obs: Observable<any>[] = [];
    splittedMultiplePaths.forEach((path) => {
      obs.push(
        this.http.get<Translation>(`/assets/i18n/${path}/${activeLang}.json`)
      );
    });

    return forkJoin([...obs]).pipe(
      map(([translation, vendor]) => {
        return { ...translation, ...vendor };
      })
    );
  }
```

2) [Example of translation files](https://github.com/yerevin/hire-me-recruitation-repo/tree/master/angular-rxjs/src/assets/i18n/auth) - you need to wrap every file with unique module prefix (eg. "auth" like below)
```json
{
  "auth": {
    "email": "Email",
  }
}
```

3) In module inside what you want to use your multiple translations define provider option with useValue containing array of translations that you want to load [Module](https://github.com/yerevin/hire-me-recruitation-repo/blob/master/angular-rxjs/src/app/modules/auth/module.ts)
```angular
providers: [{ provide: TRANSLOCO_SCOPE, useValue: ['core', 'auth'] }]
```

4) Finally - you can use your multiple translations inside html templates [Template](https://github.com/yerevin/hire-me-recruitation-repo/blob/master/angular-rxjs/src/app/modules/auth/components/login/login.component.html)
```
  <label for="email">'{{ "auth.email" | transloco }}'</label>
  <button>'{{ "core.add" | transloco }}'</button>
```


Happy coding! 🚀
