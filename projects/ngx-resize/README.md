# NgxResize

A service that emits changes of a DOM container on `resize` by utilizing [Resize Observer](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)

## Installation

```shell
npm install ngx-resize
```

## Usage

There are two approaches of using `ngx-resize`. Both approaches results an `Observable<NgxResizeResult>`

```ts
export interface NgxResizeResult {
    readonly entries: ReadonlyArray<ResizeObserverEntry>;
    readonly x: number;
    readonly y: number;
    readonly width: number;
    readonly height: number;
    readonly top: number;
    readonly right: number;
    readonly bottom: number;
    readonly left: number;
    readonly dpr: number;
}
```

### `injectNgxResize()`

If you're on Angular 14+ and are using `inject()` for **Dependency Injection**, you can use `injectNgxResize()` to grab the `Observable<NgxResizeResult>`

```ts
@Component({})
export class SomeComponent {
    readonly resizeResult$ = injectNgxResize(); // Observable<NgxResizeResult>
}
```

`injectNgxResize()` accepts a `Partial<NgxResizeOptions>` and will be merged with the default global options.

```ts
export interface NgxResizeOptions {
    box: ResizeObserverBoxOptions;
    debounce: number | { scroll: number; resize: number };
    scroll: boolean;
    offsetSize: boolean;
}

export const defaultResizeOptions: NgxResizeOptions = {
    box: 'content-box',
    scroll: false,
    offsetSize: false,
    debounce: { scroll: 50, resize: 0 },
};
```

#### With `Output`

Instead of getting the `Observable<NgxResizeResult>`, you can assign `injectNgxResize()` to an `Output` directly

```ts
@Component({})
export class SomeComponent {
    @Output() resize = injectNgxResize(); // resize emits everytime NgxResize emits
}
```

```html
<some-component (resize)="onResize($event)"></some-component>
<!-- $event is of type NgxResizeResult -->
```

### `NgxResize`

If you're not using `inject()`, you can use the `NgxResize` directive

```html
<some-component (ngxResize)="onResize($event)"></some-component>
<some-component
    (ngxResize)="onResize($event)"
    [ngxResizeOptions]="optionalOptions"
></some-component>
```

## Provide global `NgxResizeOptions`

You can use `provideNgxResizeOptions()` to provide global options for a specific Component tree. If you call `provideNgxResizeOptions()` in `bootstrapApplication()` (for Standalone) and `AppModule` (for NgModule)
then the options is truly global.

## Contributions

All contributions of any kind are welcome.
