<template>
    <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-danger">
            <div class="panel-heading">
                Items at Merchant
            </div>

            <div class="panel-body">
                <table class="table table-striped table-hover" v-if="merchant.length >= 1">
                    <thead>
                        <tr>
                            <th>Item</th>
                            <th>Price</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in merchant"
                        :class="{ danger: cash < item.price, warning: (load + item.weight) > maxLoad() }">
                            <td>
                                <img data-toggle="tooltip" data-html="true"
                                :title="getTooltip(item.title, item.level, item.weight, item.price)"
                                :src="getImage(item.type, item.level)" alt="" />
                            </td>
                            <td>{{ item.price }}$</td>
                            <td>
                                <div class="btn-group pull-right" role="group" aria-label="...">
                                    <button class="btn btn-sm btn-success"
                                    type="button"
                                    v-if="cash >= item.price && maxLoad() >= (load + item.weight)"
                                    @click="itemLocation(index, item, 'Backpack', 'Merchant')">
                                        <span class="glyphicon glyphicon-shopping-cart" aria-hidden="true"></span>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <div v-if="merchant.length <= 0">
                    The merchant has nothing to offer.
                </div>
            </div>

            <div class="panel-footer" v-if="merchant.length >= 1">
                {{ merchant.length }} Item(s)
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['merchant', 'cash', 'load', 'maxLoad'],
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
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>