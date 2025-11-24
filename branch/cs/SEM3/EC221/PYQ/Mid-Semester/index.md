<div align="center">
  <h1>🏫 National Institute of Technology Silchar</h1>
</div>

<CodeTabs :languages="[
  { name: '2023-Oct', slot: '2023-Oct' },
  { name: '2019-SEP', slot: '2019-SEP' },
  { name: '2018-SEP', slot: '2018-SEP' },
]">

<template #2018-SEP>

<details style="margin-bottom: 5px; margin-top : 10px; border: 1px solid #ccc; border-radius: 5px; padding: 8px;">
  <summary style="font-weight: bold; cursor: pointer; font-size: 1rem;">Click to view the Question Paper</summary>
  <div align="center" style="margin-top: 10px;">
    <img src="https://res.cloudinary.com/dywrfit3t/image/upload/v1763981064/Screenshot_24-Nov_16-14-12_21066_sfi4cl.png" 
         alt="Question Paper" 
         style="max-width: 90%; height: auto; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.2);" />
</div>
</details>

</template>

<template #2019-SEP>

<details style="margin-bottom: 5px; margin-top : 10px; border: 1px solid #ccc; border-radius: 5px; padding: 8px;">
  <summary style="font-weight: bold; cursor: pointer; font-size: 1rem;">Click to view the Question Paper</summary>
  <div align="center" style="margin-top: 10px;">
    <img src="https://res.cloudinary.com/dywrfit3t/image/upload/v1763919549/Screenshot_23-Nov_23-08-59_5268_e89ddq.png" 
         alt="Question Paper" 
         style="max-width: 90%; height: auto; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.2);" />
</div>
</details>

</template>

<template #2023-Oct>

<details style="margin-bottom: 5px; margin-top : 10px; border: 1px solid #ccc; border-radius: 5px; padding: 8px;">
  <summary style="font-weight: bold; cursor: pointer; font-size: 1rem;">Click to view the Question Paper</summary>
  <div align="center" style="margin-top: 10px;">
    <img src="https://res.cloudinary.com/dzgoq3ikq/image/upload/v1760767856/Screenshot_18-Oct_11-40-37_16737_jcwmmh.png" 
         alt="Question Paper" 
         style="max-width: 90%; height: auto; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.2);" />
</div>
</details>

<table style="width: 100%; border-collapse: collapse; border-color:none">
  <caption> Answer all Questions </caption>
<thead>
    <tr>
      <th>S No</th>
      <th>Questions</th>
      <th>Marks</th>
      <th>CO</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1.</td>
      <td>A digital system has four bits '<strong>ABCD</strong>' as inputs. The output <strong>Y</strong> is equal to '<strong>1</strong>' when any two adjacent bits are <strong>1</strong>, or any three or all four bits are <strong>1</strong>. Draw and simplify the <code>K-map</code>. Also, realize <strong>Y</strong> using minimum number of <strong>2-input</strong> and <strong>3-input NAND</strong> gates only.</td>
      <td>6</td>
      <td>CO1</td>
    </tr>
    <tr>
      <td>2.</td>
      <td>Simplify the following Boolean expression using <strong>Quine-McCluskey method</strong><br><code>f(A, B, C, D, E) = ∑ m(1,2,12,13,15,17,18,19,20,21,23,24,25,27,29,31)</code></td>
      <td>8</td>
      <td>CO2</td>
    </tr>
    <tr>
      <td>3.</td>
      <td>Implement the following Boolean expression using <strong>2x1 multiplexer</strong> and other necessary gates. Assume <strong>w₁</strong> as select line of multiplexer.<br><code>f = w<sub>1</sub>w<sub>2</sub> + w<sub>1</sub>w<sub>3</sub> + w̄<sub>3</sub>w̄<sub>1</sub></code></td>
      <td>4</td>
      <td>CO2<br>CO3</td>
    </tr>
    <tr>
      <td>4.</td>
      <td>Determine the <code>switching function</code> of the threshold element shown in the <strong>Figure 1</strong>.<br>[Figure shows a threshold gate with:<br>- Input <strong>A</strong> with weight <strong>-2</strong><br>- Input <strong>B</strong> with weight <strong>3</strong><br>- Input <strong>C</strong> with weight <strong>2</strong><br>- Threshold value <strong>2</strong> inside the circle]</td>
      <td>4</td>
      <td>CO5</td>
    </tr>
    <tr>
      <td>5.</td>
      <td>With proper circuit diagram explain the operation of a <code>TTL NAND gate</code> when both the inputs are at level <strong>Low</strong> and level <strong>High</strong> (show the direction of current flow). What are the <strong>fan out</strong>, <strong>noise margin</strong> and <strong>propagation delay</strong> of a standard <strong>TTL family</strong>?</td>
      <td>5 + 1</td>
      <td>CO5</td>
    </tr>
    <tr>
      <td>6.</td>
      <td>Calculate the fan out of a <code>TTL circuit</code> with the following specifications: <strong>I<sub>OL</sub>(max) = 32mA</strong>, <strong>I<sub>IL</sub>(max) = 1.6mA</strong>, <strong>I<sub>OH</sub>(max) = 400μA</strong>, <strong>I<sub>IH</sub>(max) = 10μA</strong></td>
      <td>2</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</template>

</CodeTabs>
