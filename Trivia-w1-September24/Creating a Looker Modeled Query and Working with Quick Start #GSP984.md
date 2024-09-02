# GSP984
- First Login to `Looker` with the given credentials.
In the Looker navigation menu, click Explore.

Under E-Commerce Training, click Order Items.

Select the fields below:

Under Users > Dimensions, click State.
Under Products > Dimensions, click Department.
Under Users > Measures, click Count.
Under Order Items > Measures, click Order Count.
Under Users>Dimensions, find Country and click the filter icon (filter icon) next to it to filter on the dimension.

In the Filter pane, set the Users Country filter to is equal to USA.

In the Data pane, click on the column name Users Count to sort in descending order.

Click Run and review the results data.
## Paste the following In the training_ecommerce.model file
```
connection: "bigquery_public_data_looker"

# include all the views
include: "/views/*.view"
include: "/z_tests/*.lkml"
include: "/**/*.dashboard"

datagroup: training_ecommerce_default_datagroup {
  # sql_trigger: SELECT MAX(id) FROM etl_log;;
  max_cache_age: "1 hour"
}

persist_with: training_ecommerce_default_datagroup

label: "E-Commerce Training"

explore: order_items {
  join: users {
    type: left_outer
    sql_on: ${order_items.user_id} = ${users.id} ;;
    relationship: many_to_one
  }

  join: inventory_items {
    type: left_outer
    sql_on: ${order_items.inventory_item_id} = ${inventory_items.id} ;;
    relationship: many_to_one
  }

  join: products {
    type: left_outer
    sql_on: ${inventory_items.product_id} = ${products.id} ;;
    relationship: many_to_one
  }

  join: distribution_centers {
    type: left_outer
    sql_on: ${products.distribution_center_id} = ${distribution_centers.id} ;;
    relationship: many_to_one
  }
}

# Place in `training_ecommerce` model
explore: +order_items {
  query: start_from_here{
      dimensions: [products.department, users.state]
      measures: [order_count, users.count]
      filters: [users.country: "USA"]
    }
}


explore: events {
  join: event_session_facts {
    type: left_outer
    sql_on: ${events.session_id} = ${event_session_facts.session_id} ;;
    relationship: many_to_one
  }
  join: event_session_funnel {
    type: left_outer
    sql_on: ${events.session_id} = ${event_session_funnel.session_id} ;;
    relationship: many_to_one
  }
  join: users {
    type: left_outer
    sql_on: ${events.user_id} = ${users.id} ;;
    relationship: many_to_one
  }
}
```

### Enter a `Title` for the Look report
```code
Order Items Count per State
```