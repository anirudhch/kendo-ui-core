---
title: Persist Collapsed State of Grouped Records
page_title: Persist Collapsed State of Grouped Records | Kendo UI Grid 
description: "Learn how to persist the collapsed state of grouped records in a Kendo UI Grid."
slug: howto_persist_collapsed_stateof_grouped_records_grid
---

# Persist Collapsed State of Grouped Records

The example below demonstrates how to persist the collapsed state of grouped records in a Kendo UI Grid.

###### Example

```html
  <div id="grid"></div>
    <script>
      var groups = [],
          crudServiceBaseUrl = "",
          dataSource = new kendo.data.DataSource({
            transport: {
              read:  {
                url: "http://demos.kendoui.com/service/Products",
                dataType: "jsonp"
              }
            },
            pageSize: 20,
            requestEnd: function (e) {
              if (groups.length != this.group().length) {
                var dataSourceGroups = this.group(),
                    length = groups.length;
                if (length > dataSourceGroups.length) {
                  if (dataSourceGroups.length === 0) {
                    collapsed = {};
                  } else {                        
                    for (var key in collapsed) {                            
                      if (key.indexOf(length - 1) === 0) {
                        collapsed[key] = false;
                      }
                    }
                  }
                }

                groups = this.group().slice(0);
              }
            }
          });

      $("#grid").kendoGrid({
        dataSource: dataSource,
        pageable: true,
        height: 430,
        dataBound: dataBound,
        sortable: true,
        groupable: true,
        columns: [
          "ProductName",
          { field: "UnitPrice", title: "Unit Price", format: "{0:c}", width: "100px" },
          { field: "UnitsInStock", title:"Units In Stock", width: "100px" },
          { field: "Discontinued", width: "100px" }]
      });

      var collapsed = {};

      $(function () {
        var grid = $("#grid").data("kendoGrid");
        grid.table.on("click", ".k-grouping-row .k-i-collapse, .k-grouping-row .k-i-expand", function (e) {
          var row = $(this).closest("tr"),
              groupKey = rowGroupKey(row, grid);

          if ($(this).hasClass("k-i-collapse")) {
            collapsed[groupKey] = false;
          }
          else {
            collapsed[groupKey] = true;
          }
        });
      });

      function rowGroupKey(row, grid) {
        var next = row.nextUntil("[data-uid]").next(),
            item = grid.dataItem(next.length ? next : row.next()),
            groupIdx = row.children(".k-group-cell").length,
            groups = grid.dataSource.group(),
            field = grid.dataSource.group()[groupIdx].field,
            groupValue = item[field];
        return "" + groupIdx + groupValue;
      }

      function dataBound(e) {
        var grid = this,
            groups = grid.dataSource.group();
        if (groups.length) {
          grid.tbody.children(".k-grouping-row").each(function () {
            var row = $(this),
                groupKey = rowGroupKey(row, grid);
            if (collapsed[groupKey]) {
              grid.collapseRow(row);
            }
          });
        }
      }  
    </script>  
```

## See Also

Other articles on Kendo UI Grid and how-to examples:

* [JavaScript API Reference](/api/javascript/ui/grid)
* [How to Add Cascading DropDownList Editors]({% slug howto_add_cascading_dropdown_list_editors_grid %})
* [How to Add Tooltip to Grid Cells]({% slug howto_add_tooltipto_grid_cell_record_grid %})
* [How to Copy Data from Excel]({% slug howto_copy_datafrom_excel_grid %})
* [How to Create Checkbox Filter Menu]({% slug howto_create_checkbox_filter_menu_grid %})
* [How to Customize Rows and Cells Based on Data Item Values]({% slug howto_customize_rowsand_cells_basedon_dataitem_values_grid %})
* [How to Drag and Drop Rows between Grids]({% slug howto_dragand_drop_rows_between_twogrids_grid %})
* [How to Enable ForeignKey Column Sorting by Text]({% slug howto_enable_foreignkey_sotringby_text_grid %})
* [How to Filter Array Columns Using MultiSelect]({% slug howto_filetr_array_columns_using_multiselect_grid %})
* [How to Filter Grid as You Type]({% slug howto_filter_gridas_you_type_grid %})
* [How to Implement Stable Sort in Chrome]({% slug howto_implement_stable_sortin_chrome_grid %})
* [How to Initialize Data Attribute with Detail Template]({% slug howto_initialize_data_attributewith_detail_template_grid %})
* [How to Load and Append More Records While Scrolling Down]({% slug howto_loadand_append_morerecords_while_scrollingdown_grid %})
* [How to Perform CRUD Operations with Local Storage Data]({% slug howto_perform_crud_operationswith_local_storage_data_grid %})
* [How to Persist Expanded Rows after Refresh]({% slug howto_persist_expanded_rows_afetrrefresh_grid %})
* [How to Preserve Grid State in a Cookie]({% slug howto_preserve_gridstate_inacookie_grid %})
* [How to Set Cell Color Based on ForeignKey Values]({% slug howto_set_cell_color_basedon_foreignkey_values_grid %})
* [How to Show Tooltip for Column Records]({% slug howto_show_tooltipfor_column_records_grid %})
* [How to Sort Multiple Checkbox Filter]({% slug howto_sort_multiple_checkbox_filter_grid %})
* [How to Update Toolbar Content Using MVVM Binding]({% slug howto_update_toolbar_content_using_mvvmbinding_grid %})
* [How to Use Checkboxes inside Column Menus]({% slug howto_use_checkboxes_inside_column_menu_grid %})
* [How to Use Draggable Elements with Multiselection Enabled]({% slug howto_use_draggable_elements_multiselection_enabled_grid %})
* [How to Use Grid in Kendo UI SPA Application]({% slug howto_use_gridin_kendouispa_app_grid %})
* [How to Use MultiSelect for Column Filtering]({% slug howto_use_multiselect_forcolumn_filtering_grid %})
* [How to Use Nested Chart]({% slug howto_use_nested_charts_grid %})
* [How to Use Nested Model Properties]({% slug howto_use_nested_model_properties_grid %})
* [How to Use WebAPI with Server-Side Operations]({% slug howto_use_webapi_withserverside_operations_grid %})
