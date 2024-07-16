<template>
  <div class="page">
    <AppBar :title="title" :subtitle="subtitle" class="app-bar" />
    <SubmitBar @click-done="submit" @click-cancel="cancel" />
    <main class="main-container">
      <details class="entry">
        <summary class="header" @click.prevent="toggle">
          <div class="summary-container">
            <span class="flex gap-2 font-bold text-lg">{{
              delivery?.cloned_supplier.name
            }}</span>
            <v-icon>{{ detailsIcon }}</v-icon>
          </div>
        </summary>
        <pre class="details">
          <div class="lh-normal">{{ delivery?.cloned_supplier.delivery_conditions }}</div>
        </pre>
      </details>
      <div class="flex flex-col gap-4">
        <BaseForm class="form-wrapper">
          <BaseTextArea
            class="c-white"
            variant="solo"
            :label="t('emailText')"
            name="email_text"
          >
            <template #appendIcon>
              <BaseButton
                layout="text"
                @click.stop="refsDialog.email_text?.toggleDialog"
              >
                <v-icon>mdi-36px mdi-dark mdi-file-send</v-icon>
              </BaseButton>
            </template>
          </BaseTextArea>
          <BaseDialog
            :ref="(el) => updateRefsDialog(el, 'email_text')"
            class="overlay"
            theme="light"
            @close="refsDialog.email_text?.toggleDialog"
          >
            <template #title>
              <span class="info-title">
                <span class="font-bold">
                  {{ t('emailText') }}
                </span>
                {{ t('for') }} {{ delivery?.cloned_supplier.name }}
              </span>
            </template>
            <template #description>
              <ul class="info-list">
                <template
                  v-for="(distributionInfo, index) in previousDistributionsList"
                  :key="index"
                >
                  <template
                    v-for="deliveryInfo in distributionInfo.deliveries_data"
                    :key="deliveryInfo.id"
                  >
                    <li
                      v-if="deliveryInfo.email_text"
                      class="info-item"
                      @click="copyContent($event, 'email_text')"
                    >
                      <pre>{{ deliveryInfo.email_text }}</pre>
                    </li>
                  </template>
                </template>
              </ul>
            </template>
          </BaseDialog>
          <BaseInput name="delivery_date_text" data-theme="inverted" />
          <div class="flex gap-4">
            <BaseDatePicker
              name="delivery_datetime_start"
              :placeholder="t('delivery_datetime_start')"
              clearable
            />
            <BaseDatePicker
              name="delivery_datetime_end"
              :placeholder="t('delivery_datetime_end')"
              clearable
            />
          </div>
          <BaseTextArea
            name="delivery_address"
            variant="solo"
            :label="t('deliveryAddress')"
          >
            <template #appendIcon>
              <BaseButton
                layout="text"
                @click.stop="refsDialog.delivery_address?.toggleDialog"
              >
                <v-icon>mdi-36px mdi-dark mdi-file-send</v-icon>
              </BaseButton>
            </template>
          </BaseTextArea>
          <BaseDialog
            :ref="(el) => updateRefsDialog(el, 'delivery_address')"
            class="overlay"
            theme="light"
            @close="refsDialog.delivery_address?.toggleDialog"
          >
            <template #title>
              <span class="info-title">
                <span class="font-bold">
                  {{ t('deliveryAddress') }}
                </span>
                {{ t('for') }} {{ delivery?.cloned_supplier.name }}
              </span>
            </template>
            <template #description>
              <ul class="info-list">
                <template
                  v-for="(distributionInfo, index) in previousDistributionsList"
                  :key="index"
                >
                  <template
                    v-for="deliveryInfo in distributionInfo.deliveries_data"
                    :key="deliveryInfo.id"
                  >
                    <li
                      v-if="deliveryInfo.delivery_address"
                      class="info-item"
                      @click="copyContent($event, 'delivery_address')"
                    >
                      <div>
                        {{
                          toDate(
                            distributionInfo.supplier_info?.distribution_start,
                          )
                        }}
                        {{ distributionInfo.supplier_info?.name }}
                      </div>
                      <pre class="info">{{ deliveryInfo.delivery_address }}</pre>
                    </li>
                  </template>
                </template>
              </ul>
            </template>
          </BaseDialog>
          <BaseTextArea
            name="delivery_comment"
            class="c-black text-area"
            variant="solo"
            :label="t('comment')"
          >
            <template #appendIcon>
              <BaseButton
                layout="text"
                @click.stop="refsDialog.delivery_comment?.toggleDialog"
              >
                <v-icon>mdi-36px mdi-dark mdi-file-send</v-icon>
              </BaseButton>
            </template>
          </BaseTextArea>
          <BaseDialog
            :ref="(el) => updateRefsDialog(el, 'delivery_comment')"
            class="overlay"
            theme="light"
            @close="refsDialog.delivery_comment?.toggleDialog"
          >
            <template #title>
              <span class="info-title">
                <span class="font-bold">
                  {{ t('comment') }}
                </span>
                {{ t('for') }} {{ delivery?.cloned_supplier.name }}
              </span>
            </template>
            <template #description>
              <ul class="info-list">
                <template
                  v-for="(distributionInfo, index) in previousDistributionsList"
                  :key="index"
                >
                  <template
                    v-for="deliveryInfo in distributionInfo.deliveries_data"
                    :key="deliveryInfo.id"
                  >
                    <li
                      v-if="deliveryInfo.delivery_comment"
                      class="info-item"
                      @click="copyContent($event, 'delivery_comment')"
                    >
                      <pre>{{ deliveryInfo.delivery_comment }}</pre>
                    </li>
                  </template>
                </template>
              </ul>
            </template>
          </BaseDialog>
          <BaseTextArea
            name="delivery_invoice_address"
            class="c-black"
            variant="solo"
            :label="t('billingAddress')"
          >
            <template #appendIcon>
              <BaseButton
                layout="text"
                @click.stop="refsDialog.delivery_invoice_address?.toggleDialog"
              >
                <v-icon>mdi-36px mdi-dark mdi-file-send</v-icon>
              </BaseButton>
            </template>
          </BaseTextArea>
          <BaseDialog
            :ref="(el) => updateRefsDialog(el, 'delivery_invoice_address')"
            class="overlay"
            theme="light"
            @close="refsDialog.delivery_invoice_address?.toggleDialog"
          >
            <template #title>
              <span class="info-title">
                <span class="font-bold">
                  {{ t('billingAddress') }}
                </span>
                {{ t('for') }} {{ delivery?.cloned_supplier.name }}
              </span>
            </template>
            <template #description>
              <ul class="info-list">
                <template
                  v-for="(distributionInfo, index) in previousDistributionsList"
                  :key="index"
                >
                  <template
                    v-for="deliveryInfo in distributionInfo.deliveries_data"
                    :key="deliveryInfo.id"
                  >
                    <li
                      v-if="deliveryInfo.delivery_invoice_address"
                      class="info-item"
                      @click="copyContent($event, 'delivery_invoice_address')"
                    >
                      <pre>{{ deliveryInfo.delivery_invoice_address }}</pre>
                    </li>
                  </template>
                </template>
              </ul>
            </template>
          </BaseDialog>
        </BaseForm>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import type { ComponentExposed } from 'vue-component-type-helpers'
import {
  useDisributionDeliveryBySupplierId,
  useDistributionInfoById,
  useSuppliersDeliveriesByDistributionId,
  useUpsertDistributionDelivery,
} from '~/api'
import BaseDialog from '~/components/BaseDialog.vue'
import type { Tables } from '~/types'

definePageMeta({
  middleware: 'auth',
})

const supplierId = +useRoute().params.supplierId
const distributionId = +useRoute().params.distributionId

const upsertDistributionDelivery = useUpsertDistributionDelivery()

const { data: deliveryData, suspense: deliverySuspense } =
  useDisributionDeliveryBySupplierId(distributionId, supplierId)
await deliverySuspense()

const { data: distributionList, suspense: distributionSuspense } =
  useDistributionInfoById(distributionId)
await distributionSuspense()

const { data: previousDistributions, suspense: previousDistributionsSuspense } =
  useSuppliersDeliveriesByDistributionId(supplierId)
await previousDistributionsSuspense()

const { t } = useI18n()

const lastDeliveryData = computed(() => {
  return Object.fromEntries(
    Object.entries(
      deliveryData.value?.at(0)?.delivery_data.at(-1) ?? {},
    ).filter(([key, _]) => key !== 'id'),
  )
})

type DistributionsInfo = {
  supplier_info: Tables['distributions']['Row']
  deliveries_data: Tables['distributions_suppliers_deliveries']['Row'][]
}[]

const previousDistributionsList = computed(() => {
  const distributionsInfoList: DistributionsInfo = []

  previousDistributions.value?.forEach((distributions) => {
    const result = distributions.suppliers_data.filter(
      (suppliers) => suppliers.supplier_info.id !== distributionId,
    )

    distributionsInfoList.push(...result)
  })

  return distributionsInfoList
})

const { setValues, values } = useForm({
  initialValues: {
    ...lastDeliveryData.value,
  },
})

type FormValuesKeys = keyof typeof values

const refsDialog = ref<
  Record<FormValuesKeys, ComponentExposed<typeof BaseDialog> | null>
>(
  Object.keys(values).reduce(
    (acc, key) => {
      acc[key as FormValuesKeys] = null
      return acc
    },
    {} as Record<FormValuesKeys, ComponentExposed<typeof BaseDialog> | null>,
  ),
)

const updateRefsDialog = (
  el: Element | globalThis.ComponentPublicInstance | null,
  name: FormValuesKeys,
) => {
  refsDialog.value[name] = el as unknown as ComponentExposed<typeof BaseDialog>
}

const detailsOpen = ref(false)

const currentDistribution = computed(() => distributionList.value?.at(0))

const delivery = computed(() => deliveryData.value?.at(0))

const title = computed(() =>
  distributionTitle(
    currentDistribution.value as unknown as DistributionTitleDataType,
  ),
)
const subtitle = computed(() =>
  distributionSubtitle(
    currentDistribution.value as unknown as DistributionSubtitleDataType,
  ),
)

const detailsIcon = computed(() => {
  return detailsOpen.value
    ? 'mdi mdi-36px mdi-chevron-up'
    : 'mdi mdi-36px mdi-chevron-down'
})

const submit = async () => {
  await upsertDistributionDelivery
    .mutateAsync({
      ...values,
      distribution: distributionId,
      distribution_supplier: delivery.value?.id,
    })
    .then(() => {
      cancel()
    })
}

const cancel = () =>
  backOrNavigateTo(
    `/admin/distribution/${distributionId}/buy-supplier/${supplierId}`,
  )

const toggle = (event: MouseEvent) => {
  const target = event.target as HTMLElement
  const targetDetails = target.closest('details')!
  targetDetails.open = !targetDetails.open
  detailsOpen.value = targetDetails.open

  targetDetails.scrollIntoView({
    behavior: 'smooth',
    block: 'nearest',
  })
}

const copyContent = (event: MouseEvent, name: FormValuesKeys) => {
  const target = event.target as HTMLElement
  const content = target.closest('.info-item') as HTMLElement

  if (name === 'delivery_address') {
    content.removeChild(content.childNodes[0])
  }

  setValues({
    [name]: content.innerText,
  })

  if (refsDialog.value[name] && refsDialog.value) {
    refsDialog.value[name]!.isDialogOpen = false
  }
}
</script>

<style scoped>
.page {
  --header-bg: var(--colors-yellow);
  --header-color: black;
  color: black;
}

.main-container {
  background-color: var(--colors-grayLight-1);
  padding: 2rem;
  margin-block: 2rem;

  display: flex;
  flex-direction: column;
  gap: 2rem;
  color: black;
  height: auto
}

.entry {
  padding-block: 0.5rem;
  margin-bottom: 0.5rem;

  display: flex;
  flex-direction: column;

  background-color: #fff;

  > summary {
    list-style: none;
    cursor: pointer;
  }

  &[aria-invalid='true'] {
    outline: 2px solid red;
  }

  transition: background-color 100ms;
}

.header {
  display: flex;
  justify-content: space-between;
  gap: 0.5rem;
  padding: 0.8rem;
}

.form-wrapper {
  background: var(--colors-grayLight-1);
  padding: 0;
}

.details {
  padding: 0.8rem;
  white-space: pre-wrap;
}

.summary-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5rem;
  width: 100%;
}

.input {
  height: 3rem;
}

.info-list {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-height: 60vh;
  overflow: auto;
}

.info-item {
  background: white;
  padding: 0.8rem;
  width: 50vw;
}

.info-title {
  display: flex;
  gap: 0.5rem;
}

.info {
  color: grey;
}

.icon-input {
  position: absolute;
  top: 0;
  right: 0;
}

.notification {
  margin: 2rem;
}

.dp__input {
  --dp-background-color: white;
  background-color: white;
}
</style>
