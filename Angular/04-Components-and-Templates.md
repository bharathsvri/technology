# Components and Templates

## Component metadata

```ts
@Component({
  selector: 'app-user-card',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './user-card.component.html',
})
export class UserCardComponent { }
```

## Inline vs external template

`template` string vs `templateUrl` for larger HTML.

## Styles

`styleUrls` or `styles` with encapsulation (`Emulated`, `ShadowDom`, `None`).

Next: [Data binding](05-Data-Binding.md).
