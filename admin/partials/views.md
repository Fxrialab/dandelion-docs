Template 

Hiện thị ở html

```cpp
        <table ng-table="tableParams"  show-filter="true" template-pagination="pager" class="table table-bordered table-hover table-striped">
                            <tr ng-repeat="post in $data">
                                <td data-title="'Content'" filter="{ 'content': 'text' }">
                                    {{post.content}}
                                </td>
                                <td data-title="'Like'">
                                    {{post.nLike}}
                                </td>
                                <td data-title="'Comment'">
                                    {{post.nComment}}
                                </td>
                                <td data-title="'Owner Name'"  filter="{ 'owner': 'text' }">
                                    {{post.owner}}
                                </td>
<!--                                <td data-title="'Actor Name'">
                                    {{post.actor}}
                                </td>-->
                                <td data-title="'Active'"  filter="{ 'active': 'active'}">
                                    <a href="javascript:void(0)" class="post_{{post.id}}" ng-click="active(post.recordID)">{{post.active}}</a>
                                </td>
                            </tr>
                        </table>
```
