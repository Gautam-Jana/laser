import React, { useState } from 'react';
import { 
  SdfBox, SdfBoxStack, SdfButton, SdfCard, SdfFocusPane, 
  SdfInput, SdfLink, SdfSelectSimple, SdfTab, SdfTabGroup, SfcShellAppBar 
} from '@waypoint/react-components';

interface FormData {
  region: string;
  companyCode: string;
  batchNumber: string;
  reportType: string;
  reportName: string;
  reportHold: string;
  reportId: string;
  reportAge: string;
  reportFromDate: string;
  reportToDate: string;
  reportPerPage: string;
}

interface ReportTypeOption {
  label: string;
  value: string;
}

const LaserStorageFacility: React.FC = () => {
  // State management
  const [activeTab, setActiveTab] = useState<'criteria' | 'reports' | 'audit'>('criteria');
  const [isFilterPaneOpen, setIsFilterPaneOpen] = useState(false);
  const [formData, setFormData] = useState<FormData>({
    region: '',
    companyCode: '',
    batchNumber: '',
    reportType: '',
    reportName: '',
    reportHold: '',
    reportId: '',
    reportAge: '',
    reportFromDate: '',
    reportToDate: '',
    reportPerPage: '25',
  });

  // Dropdown options
  const regionOptions: ReportTypeOption[] = [
    { label: 'North America', value: 'na' },
    { label: 'Europe', value: 'eu' },
    { label: 'Asia Pacific', value: 'ap' },
  ];

  const companyOptions: ReportTypeOption[] = [
    { label: 'Company A', value: 'compA' },
    { label: 'Company B', value: 'compB' },
    { label: 'Company C', value: 'compC' },
  ];

  const reportTypeOptions: ReportTypeOption[] = [
    { label: 'Deferred', value: '1' },
    { label: 'Generic Letter', value: '2' },
    { label: 'Quarter', value: '3' },
    { label: 'Quarter w/PR', value: '4' },
    { label: 'Wrap', value: '5' },
  ];

  const reportHoldOptions: ReportTypeOption[] = [
    { label: 'No', value: '1' },
    { label: 'Yes', value: '2' },
  ];

  // Handlers
  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSelectChange = (name: keyof FormData) => (value: string) => {
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
    // API call would go here
    alert('Report criteria submitted successfully!');
  };

  const handleClear = () => {
    setFormData({
      region: '',
      companyCode: '',
      batchNumber: '',
      reportType: '',
      reportName: '',
      reportHold: '',
      reportId: '',
      reportAge: '',
      reportFromDate: '',
      reportToDate: '',
      reportPerPage: '25',
    });
  };

  const handleTabChange = (tab: 'criteria' | 'reports' | 'audit') => {
    setActiveTab(tab);
  };

  // Format current date for display
  const currentDate = new Date();
  const formattedDate = currentDate.toLocaleDateString('en-US', {
    month: '2-digit',
    day: '2-digit',
    year: 'numeric'
  });
  const formattedTime = currentDate.toLocaleTimeString('en-US', {
    hour: '2-digit',
    minute: '2-digit',
    hour12: true
  });

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 to-black text-white p-4">
      <SfcShellAppBar size="small" className="bg-gray-800">
        Laser Storage Facility
      </SfcShellAppBar>

      <div className="max-w-7xl mx-auto mt-8">
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <SdfSelectSimple
            className="w-full rounded-lg border border-gray-600 shadow-lg bg-gray-800 text-white"
            label="Region"
            filterable
            items={regionOptions}
            value={formData.region}
            onSdfChange={(e) => handleSelectChange('region')(e.detail.value)}
          />
          
          <SdfSelectSimple
            className="w-full rounded-lg border border-gray-600 shadow-lg bg-gray-800 text-white"
            label="Company Code"
            filterable
            items={companyOptions}
            value={formData.companyCode}
            onSdfChange={(e) => handleSelectChange('companyCode')(e.detail.value)}
          />
        </div>

        <SdfTabGroup className="mb-8">
          <SdfTab 
            active={activeTab === 'criteria'} 
            icon="ext-mdi-event-available"
            onClick={() => handleTabChange('criteria')}
          >
            Selection Criteria
          </SdfTab>
          <SdfTab 
            active={activeTab === 'reports'} 
            icon="ext-mdi-inventory"
            onClick={() => handleTabChange('reports')}
          >
            View Reports
          </SdfTab>
          <SdfTab 
            active={activeTab === 'audit'} 
            icon="base-gift"
            onClick={() => handleTabChange('audit')}
          >
            Audit Trail
          </SdfTab>
        </SdfTabGroup>

        <SdfCard className="bg-gray-800/90 backdrop-blur-lg shadow-2xl rounded-xl p-6 border border-gray-700">
          <div className="flex justify-between items-center mb-6">
            <h2 className="text-xl font-bold">Report Criteria</h2>
            <div className="text-sm text-gray-400">
              {formattedTime} {formattedDate}
            </div>
          </div>

          {activeTab === 'criteria' && (
            <form onSubmit={handleSubmit}>
              <div className="mb-6 flex justify-end">
                <SdfButton 
                  id="open-button"
                  type="button"
                  onClick={() => setIsFilterPaneOpen(true)}
                  className="bg-blue-600 hover:bg-blue-700 transition-colors"
                >
                  Filter
                </SdfButton>
              </div>

              <SdfFocusPane
                closeable
                spacing="loose"
                heading="Selection Criteria"
                pane-type="anchored"
                id="demo-component"
                visible={isFilterPaneOpen}
                onSdfDismiss={() => setIsFilterPaneOpen(false)}
              >
                <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 p-4">
                  <div className="sdf-box">
                    <label className="text-gray-300">* Company: {formData.companyCode || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">* Batch: {formData.batchNumber || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt Name: {formData.reportName || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt ID: {formData.reportId || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt Type: {formData.reportType || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt Hold: {formData.reportHold || 'ALL'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt Age: {formData.reportAge || 'N/A'}</label>
                  </div>
                  <div className="sdf-box">
                    <label className="text-gray-300">Rpt Fro Data: {formData.reportFromDate || 'N/A'}</label>
                  </div>
                </div>
              </SdfFocusPane>

              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
                <SdfInput
                  label="Batch Number"
                  name="batchNumber"
                  value={formData.batchNumber}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report Name"
                  name="reportName"
                  value={formData.reportName}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report Id"
                  name="reportId"
                  value={formData.reportId}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfSelectSimple
                  label="Report Type"
                  items={reportTypeOptions}
                  value={formData.reportType}
                  onSdfChange={(e) => handleSelectChange('reportType')(e.detail.value)}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfSelectSimple
                  label="Report Hold"
                  items={reportHoldOptions}
                  value={formData.reportHold}
                  onSdfChange={(e) => handleSelectChange('reportHold')(e.detail.value)}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report Age (Days)"
                  name="reportAge"
                  type="number"
                  value={formData.reportAge}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report From Date"
                  name="reportFromDate"
                  type="date"
                  value={formData.reportFromDate}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report To Date"
                  name="reportToDate"
                  type="date"
                  value={formData.reportToDate}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
                
                <SdfInput
                  label="Report Per Page"
                  name="reportPerPage"
                  type="number"
                  value={formData.reportPerPage}
                  onChange={handleInputChange}
                  className="w-full bg-gray-700 border-gray-600 text-white"
                />
              </div>

              <div className="flex flex-wrap justify-start gap-4 pt-4 border-t border-gray-700">
                <SdfButton 
                  type="submit"
                  emphasis="primary"
                  className="bg-green-600 hover:bg-green-700 px-6 py-3 transition-colors"
                  icon="action-upload"
                >
                  Submit
                </SdfButton>
                
                <SdfButton 
                  type="button"
                  onClick={handleClear}
                  emphasis="primary"
                  className="bg-gray-600 hover:bg-gray-700 px-6 py-3 transition-colors"
                  icon="action-close"
                >
                  Clear
                </SdfButton>
                
                <SdfButton 
                  type="button"
                  emphasis="primary"
                  className="bg-blue-600 hover:bg-blue-700 px-6 py-3 transition-colors"
                  icon="action-help"
                >
                  Help
                </SdfButton>
              </div>
            </form>
          )}

          {activeTab === 'reports' && (
            <div className="py-12 text-center">
              <h3 className="text-xl font-semibold mb-4">View Reports</h3>
              <p className="text-gray-400">Select criteria and submit to view generated reports</p>
            </div>
          )}

          {activeTab === 'audit' && (
            <div className="py-12 text-center">
              <h3 className="text-xl font-semibold mb-4">Audit Trail</h3>
              <p className="text-gray-400">Audit logs will appear here</p>
            </div>
          )}
        </SdfCard>

        <div className="flex justify-center gap-8 mt-8 text-sm">
          <SdfLink className="text-blue-400 hover:text-blue-300 transition-colors">PRIVACY</SdfLink>
          <SdfLink className="text-blue-400 hover:text-blue-300 transition-colors">LEGAL</SdfLink>
        </div>
      </div>
    </div>
  );
};

export default LaserStorageFacility;
