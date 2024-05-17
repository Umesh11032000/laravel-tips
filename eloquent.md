# Using `whereHasMorph` for Polymorphic Relationships in Laravel

## Explanation

In Laravel, polymorphic relationships allow a model to belong to more than one other model on a single association. The `whereHasMorph` method is useful when querying models that have a polymorphic relationship. It filters the results based on the existence of a related polymorphic model and further refines the query based on conditions applied to that polymorphic model.

## Example

Consider a scenario where you have a polymorphic relationship with `UserActivity` being the parent model and `Note` and `Journal` being the child models. You want to search through user activities based on the content of these related models. The `whereHasMorph` method allows you to filter `UserActivity` records where the related `Note` or `Journal` contains a specific search term.

Here's how you can do it:

```php
// Get search query
$search = $request->search;

// Get all user activities
$activities = UserActivity::query()
    ->when($search != '', function ($query) use ($search) {
        $query->whereHasMorph('activityable', [Note::class, Journal::class], function (Builder $query) use ($search) {
            $query->where('body', 'like', '%' . $search . '%');
        });
    })
    ->paginate(10);
```