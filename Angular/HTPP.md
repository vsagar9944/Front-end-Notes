# HTTP

URLSearchParams

```
this.http.get(url, {search: search}).subscribe(res => console.log(res.json()));
```





## Grid Provided Components

The grid comes with pre-registered components that can be used. Each component provided by the grid starts with the namespaces 'ag' to minimise naming conflicts with user provided components. The full list of grid provided components are in the table below.

| Date Inputs                     |                                                              |
| ------------------------------- | ------------------------------------------------------------ |
| agDateInput                     | Default date input used by filters.                          |
| Column Headers                  |                                                              |
| agColumnHeader                  | Default column header.                                       |
| agColumnHeaderGroup             | Default column group header.                                 |
| Column Filters                  |                                                              |
| agSetColumnFilter               | Set filter (default when using ag-Grid Enterprise).          |
| agTextColumnFilter              | Simple text filter (default when using ag-Grid Free).        |
| agNumberColumnFilter            | Number filter.                                               |
| agDateColumnFilter              | Date filter.                                                 |
| Floating Filters                |                                                              |
| agSetColumnFloatingFilter       | Floating set filter.                                         |
| agTextColumnFloatingFilter      | Floating text filter.                                        |
| agNumberColumnFloatingFilter    | Floating number filter.                                      |
| agDateColumnFloatingFilter      | Floating date filter.                                        |
| Cell Renderers                  |                                                              |
| agAnimateShowChangeCellRenderer | Cell renderer that animates value changes.                   |
| agAnimateSlideCellRenderer      | Cell renderer that animates value changes.                   |
| agGroupCellRenderer             | Cell renderer for displaying group information.              |
| agLoadingCellRenderer           | Cell editor for loading row when using Enterprise row model. |
| Overlays                        |                                                              |
| agLoadingOverlay                | Loading overlay.                                             |
| agNoRowsOverlay                 | No rows overlay.                                             |
| Cell Editors                    |                                                              |
| agTextCellEditor                | Text cell editor.                                            |
| agSelectCellEditor              | Select cell editor.                                          |
| agRichSelectCellEditore         | Rich select editor.                                          |
| agPopupTextCellEditor           | Popup text cell editor.                                      |
| agPopupSelectCellEditor         | Popup select cell editor.                                    |
| agLargeTextCellEditor           | Large text cell editor.                                      |
| Master Detail                   |                                                              |
| agDetailCellRenderere           | Detail panel for master / detail grid.                       |

### Overriding Grid Components

It is also possible to override components. Where the grid uses a default value, this means the override component will be used instead. The default components, where overriding makes sense, are as follows:

- **agDateInput**: To change the default date selection across all filters.
- **agColumnHeader**: To change the default column header across all columns.
- **agColumnGroupHeader**: To change the default column group header across all columns.
- **agLoadingCellRenderer**: To change the default loading cell renderer for Enterprise Row Model.
- **agLoadingOverlay**: To change the default 'loading' overlay.
- **agNoRowsOverlay**: To change the default loading 'no rows' overlay.
- **agTextCellEditor**: To change the default text cell editor.
- **agDetailCellRenderer**: To change the default detail panel for master / detail grids.