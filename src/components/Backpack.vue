<template>
    <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-success">
            <div class="panel-heading">
                Items at Backpack
            </div>

            <div class="panel-body">
                <table class="table table-striped table-hover" v-if="backpack.length >= 1">
                    <thead>
                        <tr>
                            <th>Item</th>
                            <th>Weight</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in backpack"
                        :class="{ danger: level < item.level, warning: findItem(character, item.type) }">
                            <td>
                                <img data-toggle="tooltip" data-html="true"
                                :title="getTooltip(item.title, item.level, item.weight, item.price)"
                                :src="getImage(item.type, item.level)" alt="" />
                            </td>
                            <td>{{ item.weight }}kg</td>
                            <td>
                                <div class="btn-group pull-right" role="group" aria-label="...">
                                    <button class="btn btn-sm btn-info"
                                    type="button"
                                    v-if="level >= item.level && !findItem(character, item.type)"
                                    @click="itemLocation(index, item, 'Character', 'Backpack')">
                                        <span class="glyphicon glyphicon-user" aria-hidden="true"></span>
                                    </button>

                                    <button class="btn btn-sm btn-danger"
                                    type="button"
                                    @click="itemLocation(index, item, 'Merchant', 'Backpack')">
                                        <span class="glyphicon glyphicon-piggy-bank" aria-hidden="true"></span>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <div v-if="backpack.length <= 0">
                    Your Backpack slot is empty.
                </div>
            </div>

            <div class="panel-footer" v-if="backpack.length >= 1">
                {{ backpack.length }} Item(s)
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['backpack', 'character', 'level'],
        methods: {
            itemLocation(index, item, newLocation, oldLocation) {
                this.$emit('addItem', { item, newLocation });
                this.$emit('removeItem', { index, item, oldLocation });
            },
            getImage(type, level) {
                var url = '/assets/' + type + '/' + level + '.png';
                return url;
            },
            getTooltip(title, level, weight, price) {
                return '<p><strong>' + title + '</strong><br/><em>Level ' + level + '</em></p><p>' + weight + 'kg | ' + price + '$</p>';
            },
            findItem(items, item) {
                for (var i=0; i<items.length; i++) {
                    if(items[i].type === item) {
                        return true;
                    }
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>