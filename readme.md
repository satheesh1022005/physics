import React, { useState } from 'react';

function App() {
  const [f, setF] = useState([]);

  const [sets, setSets] = useState([...Array(9)].map(() => ({})));

  const handleInputChange = (index, field, value) => {
    setSets(prevSets => {
      const updatedSets = [...prevSets];
      updatedSets[index][field] = value;
      return updatedSets;
    });
  };

  const calculateResults = () => {
    const updatedSets = sets.map((set) => {
      const n1Value = parseFloat(set.n1);
      const m1Value = parseFloat(set.m1);
      const result = m1Value * 0.01 + n1Value;
      return {
        ...set,
        result: isNaN(result) ? '' : result.toFixed(2),
      };
    });

    setSets(updatedSets);

    const fArray = updatedSets.map((set) => set.result);
    setF(fArray);

    const n = fArray.length;
    const updatedOutputs = [];

    for (let i = 0; i < n - 2; i++) {
      const t = Math.abs(fArray[i] - fArray[i + 2]);
      updatedOutputs[i] = t;
    }

    setF(updatedOutputs);
  };

  return (
    <div>
      <nav class="bg-primary pb-1" id="head">
        <h1 class="text-white">Air wedge</h1>
      </nav>
      <div class="row  p-0" id="table">
        <div class="col-3 p-0">
          <div class="col-12 br w-100 p-0">
            <h6>ORDER OF THE BAND</h6>
          </div>
          <div class="col-12 br w-100 p-0">n</div>
          <div class="col-12 br w-100 p-0">n+05</div>
          <div class="col-12 br w-100 p-0">n+10</div>
          <div class="col-12 br w-100 p-0">n+15</div>
          <div class="col-12 br w-100 p-0">n+20</div>
          <div class="col-12 br w-100 p-0">n+25</div>
          <div class="col-12 br w-100 p-0">n+30</div>
          <div class="col-12 br w-100 p-0">n+35</div>
          <div class="col-12 br w-100 p-0">n+4 br0</div>
        </div>

        <div className="col-6 br">
          <h6>MICROSCOPE READING (10^-2 M)</h6>
          <div className="row">
            <div className="col-4 br p">M.S.R</div>
            <div className="col-4 br p">V.S.C</div>
            <div className="col-4 br p">C.R</div>
          </div>
          {sets.map((set, index) => (
            <div className="row" key={index}>
              <div className="col-4 br p-0">
                <input
                  type="number"
                  placeholder="Type a number"
                  value={set.n1 || ''}
                  onChange={(e) => handleInputChange(index, 'n1', e.target.value)}
                />
              </div>
              <div className="col-4 br p-0">
                <input
                  type="number"
                  placeholder="Type a number"
                  value={set.m1 || ''}
                  onChange={(e) => handleInputChange(index, 'm1', e.target.value)}
                />
              </div>
              <span className="col-4 br p-0">{set.result || ''}</span>
            </div>
          ))}
          <button onClick={calculateResults}>Calculate</button>
        </div>

        <div class="col-3 p-0">
          <div class="col-12 br w-100 p-0">
            <h6>Distance Between 10 Bands (10β)</h6>
          </div>
          {f.map((value, index) => (
            <div class="col-12 br w-100 p-0" key={index}>
              {value}
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

export default App;
